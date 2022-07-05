<img align="right" src="https://github.com/ada-school/module-template/blob/main/ada.png">


## Android REST API with JWT

Implement an Interceptor to add a JWT to the requests using Retrofit and SharedPreferences to store the token locally .

**Learning Objectives**

- Explain how Interceptos work.
- Implement an Interceptor to a JWT to the request using Retrofit.


**Main Topics**

* Interceptor.
* JWT.
* Retrofit.
* SharedPreferences.



## Codelab 🧪

🗣️ "I hear and I forget I see and I remember I do and I understand." Confucius



### Part 1: Define the Storage Interface  and implment the SharedPreferences version

1. Create a new packaged called *storage*
2. Create a new interface called *Storage* and define the common functionality to store the token info on the local preferences:
   ```kotlin
      interface Storage {

          fun saveToken(token: String)

          fun getToken(): String?

          fun clear()
      }
   ```
3. Create a new file called *Constants.kt* and add the following constants:
   ```kotlin
      const val SHARED_PREFERENCES_FILE_NAME = "my_prefs"
      const val TOKEN_KEY= "token_key"
   ``` 
4. Implement the *Storage* interface using the *SharedPreferences*:
   ```kotlin
      class SharedPreferencesStorage(private val sharedPreferences: SharedPreferences) :Storage {

          override fun saveToken(token: String) {
              sharedPreferences.edit()
                  .putString(TOKEN_KEY, token)
                  .apply()
          }

          override fun getToken(): String? {
              return sharedPreferences.getString(TOKEN_KEY, "")
          }

          override fun clear() {
              sharedPreferences.edit().clear().apply()
          }

      }
   ```
   
### Part 2: Implement the JWT Interceptor

1. Implement the JWTInterceptor and inject the *Storage* via the constructor to retrieve the token:
   ```kotlin
      class JWTInterceptor(private val tokenStorage: TokenStorage) : Interceptor {

          override fun intercept(chain: Interceptor.Chain): Response {
              val request = chain.request().newBuilder()
              val token = tokenStorage.getToken()
              if (token != null) {
                  request.addHeader("Authorization", "Bearer $token")
              }
              return chain.proceed(request.build())
          }

      }
   ```
2. Configure the *Retrofit* instance adding the JWTInterceptor:
   ```kotlin
        val loggingInterceptor = HttpLoggingInterceptor()
           loggingInterceptor.level = HttpLoggingInterceptor.Level.BODY
        val okHttpClient = OkHttpClient.Builder()
            .addInterceptor(loggingInterceptor)
            .addInterceptor(jwtInterceptor)
            .writeTimeout(0, TimeUnit.MILLISECONDS)
            .readTimeout(2, TimeUnit.MINUTES)
            .connectTimeout(1, TimeUnit.MINUTES).build()

        val gson = GsonBuilder()
            .setDateFormat("yyyy-MM-dd'T'HH:mm:ssX")
            .create()

        return Retrofit.Builder()
            .baseUrl("https://dog.ceo/api/")
            .client(okHttpClient)
            .addConverterFactory(GsonConverterFactory.create(gson))
            .addCallAdapterFactory(CoroutineCallAdapterFactory())
            .build()
   ```
### Challenge Yourself: Implement a more secure Storage that encrypts the information saved.

1. Create another implementation of the *Storage* called *EncryptedStorage* that uses encryption to save the data with more security.

   ***Tip***: Find a library that helps you achieve the encryption inside the SharedPreferences or to encrypt and decrypt each data field directly.
