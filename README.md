<img align="right" src="https://github.com/ada-school/module-template/blob/main/ada.png">


## Android REST API with JWT

Implement an Interceptor to add a JWT to the requests using Retrofit and SharedPreferences to store the token locally .

**Learning Objectives**

- Explain how Interceptos work.
- Implement an Interceptor to a JWT to request with Retrofit.


**Main Topics**

* Interceptor.
* JWT.
* Retrofit.
* SharedPreferences.



## Codelab 🧪

🗣️ "I hear and I forget I see and I remember I do and I understand." Confucius



### Part 1: Define the Storage Interface  and implment the SharedPreferences version:

1. Create a new packaged called *storage*
2. Create a new Interface called *Storage* and define the common functionality to store the Token info on the local preferences:
   ```kotlin
      interface Storage {

          fun saveToken(token: String)

          fun getToken(): String?

          fun clear()
      }
   ```
3. Implement the Storage interface using the SharedPreferences inside:
   ```kotlin
      class SharedPreferencesLocalStorage(private val sharedPreferences: SharedPreferences) :Storage {

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

### Challenge Yourself: Implement a mechanism to support Application tokens

1. Implement a new method in the *AuthController* that receives an encrypted secret and verify that the secret is the
   same that you have locally using a new environment variable. If the secret match then you will generate a token that
   will allow the server to have *ADMIN* role token for 10 minutes.

   ***Tip***: Divide this problem into smaller problems. Once you solve each problem test your solution and only continue
   if it works.
