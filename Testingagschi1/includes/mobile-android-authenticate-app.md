
1. 在 Android Studio 的 [專案總管]  中，開啟 ToDoActivity.java 檔案，並新增下列 import 陳述式。

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
2. 將下列方法加入至 **ToDoActivity** 類別：

        // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;
 
        private void authenticate() {
            // Login using the Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }
         
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check the request code matches the one we send in the login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check the error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    這會建立新的方法來處理驗證程序。 使用者透過 Google 登入來驗證。 將出現對話方塊來顯示已驗證使用者的識別碼。 必須通過驗證才能繼續。

    > [!NOTE]
    > 如果您使用的身分識別提供者不是 Google，請將傳給上述 **login** 方法的值變更為下列其中一個：_MicrosoftAccount_、_Facebook_、_Twitter_ 或 _windowsazureactivedirectory_。

3. 在 **OnCreate`MobileServiceClient` 方法中，在具現化**  物件的程式碼後面加入下列這一行程式碼。

        authenticate();

    此呼叫會啟動驗證程序。
4. 將 **onCreate** 方法中 `authenticate();` 後面的其餘程式碼移至新的 **createTable** 方法。 它看起來如下：

        private void createTable() {

            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load the items from Azure.
            refreshItemsFromTable();
        }

5. 將 _RedirectUrlActivity_ 的下列程式碼片段新增至 _AndroidManifest.xml_ 以確保重新導向可運作。
 
        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

6.  將 redirectUriScheme 新增至 Android 應用程式的 _build.gradle_。
 
        android {
            buildTypes {
                release {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
                debug {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
            }
        }

7. 將 com.android.support:customtabs:23.0.1 新增至 build.gradle 中的相依性：

      dependencies {        // ...        compile 'com.android.support:customtabs:23.0.1'    }

8. 在 [執行] 功能表中，按一下 [執行應用程式] 來啟動應用程式，並以您選擇的身分識別提供者登入。

成功登入後，應用程式應會正確無誤地執行，而且您應能夠查詢後端服務並更新資料。
