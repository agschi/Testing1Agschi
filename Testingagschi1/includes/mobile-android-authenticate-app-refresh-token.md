我們的權杖快取應該只在簡單案例中使用，在權杖過期或被撤銷時會有什麼影響？ 當應用程式停止運作的時候，那麼權杖可能過期了。 這表示權杖快取會失效。 實際執行應用程式時，權杖也可能過期。 HTTP 狀態碼 401 的結果為「未經授權」。 

我們必須能夠偵測到過期的權杖，並加以重新整理。 若要這麼做，我們可以使用 [Android 用戶端程式庫](http://dl.windowsazure.com/androiddocs/)的 [ServiceFilter](http://dl.windowsazure.com/androiddocs/com/microsoft/windowsazure/mobileservices/ServiceFilter.html)。

在本節中，您將會對 ServiceFilter 進行定義，它會偵測 HTTP 狀態碼 401 並觸發權杖和權杖快取的更新。 除此之外，ServiceFilter 會在進行驗證期間阻擋其他外來要求，如此一來這些要求即可使用更新後的權杖。

1. 開啟 ToDoActivity.java 檔案，並新增下列 import 陳述式：
   
        import java.util.concurrent.atomic.AtomicBoolean;
        import java.util.concurrent.ExecutionException;
   
        import com.microsoft.windowsazure.mobileservices.MobileServiceException;
2. 在 `ToDoActivity` 類別中新增下列成員： 
   
        public boolean bAuthenticating = false;
        public final Object mAuthenticationLock = new Object();
   
    這會用來協助使用者驗證的同步作業。 我們只想進行一次驗證。 任何在驗證期間發生的呼叫都應等待及使用正進行驗證的新權杖。
3. 在 ToDoActivity.java 檔案中，將下列方法加入 ToDoActivity 類別，這將會在驗證進行時阻擋其他執行緒上的外來呼叫。
   
        /**
         * Detects if authentication is in progress and waits for it to complete. 
         * Returns true if authentication was detected as in progress. False otherwise.
         */
        public boolean detectAndWaitForAuthentication()
        {
            boolean detected = false;
            synchronized(mAuthenticationLock)
            {
                do
                {
                    if (bAuthenticating == true)
                        detected = true;
                    try
                    {
                        mAuthenticationLock.wait(1000);
                    }
                    catch(InterruptedException e)
                    {}
                }
                while(bAuthenticating == true);
            }
            if (bAuthenticating == true)
                return true;
   
            return detected;
        }
4. 在 ToDoActivity.java 檔案中，將下列方法加入 ToDoActivity 類別中。 此方法會觸發等待動作，然後在驗證完成時更新外來要求上的權杖。 

        /**
         * Waits for authentication to complete then adds or updates the token 
         * in the X-ZUMO-AUTH request header.
         * 
         * @param request
         *            The request that receives the updated token.
         */
        private void waitAndUpdateRequestToken(ServiceFilterRequest request)
        {
            MobileServiceUser user = null;
            if (detectAndWaitForAuthentication())
            {
                user = mClient.getCurrentUser();
                if (user != null)
                {
                    request.removeHeader("X-ZUMO-AUTH");
                    request.addHeader("X-ZUMO-AUTH", user.getAuthenticationToken());
                }
            }
        }


1. 在 ToDoActivity.java 檔案中，更新 ToDoActivity 類別的 `authenticate` 方法，讓其可接受布林參數，以允許強制重新整理權杖和權杖快取。 當驗證完成後，我們也必須通知所有遭阻擋的執行緒，使其可以取得新的權杖。
   
        /**
         * Authenticates with the desired login provider. Also caches the token. 
         * 
         * If a local token cache is detected, the token cache is used instead of an actual 
         * login unless bRefresh is set to true forcing a refresh.
         * 
         * @param bRefreshCache
         *            Indicates whether to force a token refresh. 
         */
        private void authenticate(boolean bRefreshCache) {
   
            bAuthenticating = true;
   
            if (bRefreshCache || !loadUserTokenCache(mClient))
            {
                // New login using the provider and update the token cache.
                mClient.login(MobileServiceAuthenticationProvider.MicrosoftAccount,
                        new UserAuthenticationCallback() {
                            @Override
                            public void onCompleted(MobileServiceUser user,
                                    Exception exception, ServiceFilterResponse response) {
   
                                synchronized(mAuthenticationLock)
                                {
                                    if (exception == null) {
                                        cacheUserToken(mClient.getCurrentUser());
                                        createTable();
                                    } else {
                                        createAndShowDialog(exception.getMessage(), "Login Error");
                                    }
                                    bAuthenticating = false;
                                    mAuthenticationLock.notifyAll();
                                }
                            }
                        });
            }
            else
            {
                // Other threads may be blocked waiting to be notified when 
                // authentication is complete.
                synchronized(mAuthenticationLock)
                {
                    bAuthenticating = false;
                    mAuthenticationLock.notifyAll();
                }
                createTable();
            }
        }   
2. 在 ToDoActivity.java 檔案中，針對 ToDoActivity 類別內的新 `RefreshTokenCacheFilter` 類別新增此程式碼：
   
        /**
        * The RefreshTokenCacheFilter class filters responses for HTTP status code 401. 
         * When 401 is encountered, the filter calls the authenticate method on the 
         * UI thread. Out going requests and retries are blocked during authentication. 
         * Once authentication is complete, the token cache is updated and 
         * any blocked request will receive the X-ZUMO-AUTH header added or updated to 
         * that request.   
         */
        private class RefreshTokenCacheFilter implements ServiceFilter {
   
            AtomicBoolean mAtomicAuthenticatingFlag = new AtomicBoolean();                     
   
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(
                    final ServiceFilterRequest request, 
                    final NextServiceFilterCallback nextServiceFilterCallback
                    )
            {
                // In this example, if authentication is already in progress we block the request
                // until authentication is complete to avoid unnecessary authentications as 
                // a result of HTTP status code 401. 
                // If authentication was detected, add the token to the request.
                waitAndUpdateRequestToken(request);
   
                // Send the request down the filter chain
                // retrying up to 5 times on 401 response codes.
                ListenableFuture<ServiceFilterResponse> future = null;
                ServiceFilterResponse response = null;
                int responseCode = 401;
                for (int i = 0; (i < 5 ) && (responseCode == 401); i++)
                {
                    future = nextServiceFilterCallback.onNext(request);
                    try {
                        response = future.get();
                        responseCode = response.getStatus().getStatusCode();
                    } catch (InterruptedException e) {
                       e.printStackTrace();
                    } catch (ExecutionException e) {
                        if (e.getCause().getClass() == MobileServiceException.class)
                        {
                            MobileServiceException mEx = (MobileServiceException) e.getCause();
                            responseCode = mEx.getResponse().getStatus().getStatusCode();
                            if (responseCode == 401)
                            {
                                // Two simultaneous requests from independent threads could get HTTP status 401. 
                                // Protecting against that right here so multiple authentication requests are
                                // not setup to run on the UI thread.
                                // We only want to authenticate once. Requests should just wait and retry 
                                // with the new token.
                                if (mAtomicAuthenticatingFlag.compareAndSet(false, true))                                                                                                      
                                {
                                    // Authenticate on UI thread
                                    runOnUiThread(new Runnable() {
                                        @Override
                                        public void run() {
                                            // Force a token refresh during authentication.
                                            authenticate(true);
                                        }
                                    });
                                }
   
                                // Wait for authentication to complete then update the token in the request. 
                                waitAndUpdateRequestToken(request);
                                mAtomicAuthenticatingFlag.set(false);                                                  
                            }
                        }
                    }
                }
                return future;
            }
        }

    此服務篩選器將會檢查每個 HTTP 狀態碼 401「未經驗證」的回應。 如果出現 401，UI 執行緒上將會設定新的登入要求以取得新權杖。 其他呼叫都會被封鎖，直到登入完成，或已嘗試失敗 5 次為止。 如果取得新權杖，觸發 401 的要求將會以新權杖重新執行，而所有遭阻擋的呼叫也會使用新的權杖重新執行。 

1. 在 ToDoActivity.java 檔案中，針對 ToDoActivity 類別內的新 `ProgressFilter` 類別新增此程式碼：
   
        /**
        * The ProgressFilter class renders a progress bar on the screen during the time the App is waiting for the response of a previous request.
        * the filter shows the progress bar on the beginning of the request, and hides it when the response arrived.
        */
        private class ProgressFilter implements ServiceFilter {
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback nextServiceFilterCallback) {
   
                final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
   
                runOnUiThread(new Runnable() {
   
                    @Override
                    public void run() {
                        if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
                    }
                });
   
                ListenableFuture<ServiceFilterResponse> future = nextServiceFilterCallback.onNext(request);
   
                Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
                    @Override
                    public void onFailure(Throwable e) {
                        resultFuture.setException(e);
                    }
   
                    @Override
                    public void onSuccess(ServiceFilterResponse response) {
                        runOnUiThread(new Runnable() {
   
                            @Override
                            public void run() {
                                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.GONE);
                            }
                        });
   
                        resultFuture.set(response);
                    }
                });
   
                return resultFuture;
            }
        }
   
    此篩選器會在要求開始時顯示進度列，並在回應到達時隱藏。
2. 在 ToDoActivity.java 檔案中，將 `onCreate` 方法更新的方式如下：
   
        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
   
            setContentView(R.layout.activity_to_do);
            mProgressBar = (ProgressBar) findViewById(R.id.loadingProgressBar);
   
            // Initialize the progress bar
            mProgressBar.setVisibility(ProgressBar.GONE);
   
            try {
                // Create the Mobile Service Client instance, using the provided
                // Mobile Service URL and key
                mClient = new MobileServiceClient(
                        "https://<YOUR MOBILE SERVICE>.azure-mobile.net/",
                        "<YOUR MOBILE SERVICE KEY>", this)
                           .withFilter(new ProgressFilter())
                           .withFilter(new RefreshTokenCacheFilter());
   
                // Authenticate passing false to load the current token cache if available.
                authenticate(false);
   
            } catch (MalformedURLException e) {
                createAndShowDialog(new Exception("Error creating the Mobile Service. " +
                    "Verify the URL"), "Error");
            }
        }

       In this code, `RefreshTokenCacheFilter` is used in addition to `ProgressFilter`. Also during `onCreate` we want to load the token cache. So `false` is passed in to the `authenticate` method.




<!--HONumber=Jan17_HO3-->


