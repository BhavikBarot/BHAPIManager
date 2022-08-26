<p align="center">
  <img src="https://github.com/BhavikBarot/BHTextFieldManager/blob/master/res/BHTextFieldManagerIcon.png" alt="Icon"/>
</p>
<H1 align="center">BHAPIManager</H1>

#### Key Features

1) `Call APIs in easy way without any extra setup.`

2) `Easy to use and developer friendly`

3) `It works with Codable and Encodable pattern to make model mapping easier.`

4) `Call an API with Router pattern to avoid API management issues.`

5) `Multiple options are available to call API. (E.g URL, Request, Dictionary, Extra Headers)`

Installation
==========================
#### Swift Package Installation

[SwiftPackageManager]:= 'https://github.com/BhavikBarot/BHAPIManager' package URL

Implementation
==========================

**Configuration**

This will set the Base URL for you API call. (NOTE:- This will be only used when Router pattern is used)
```
API.configure(baseURL: URL(string: "https://github.com/BhavikBarot/BHAPIManager/api/V3.1/")!)
```

This will enable the log printing for the request. By detault it is false.
```
API.logs(enabled: true)
```

To set the default authorisation and refresh token detail if any to refresh the token after the token expire or invalidated.
```
API.configure(authorisation: .setAuthorisation(withAuthToken: "Bearer Token", andRefreshToken: "Token"))
```

To set the request retryer for unauthorisation. It will refresh the token details and resume the previous API by creating new session.
```
API.configure(RequestRetryer: .request(withURL: URL(string: "URL")!, andParam: [:]), andResponseForRetryer: .responseValues(authTokenKey: "authTokenKey", refreshTokenKey: "refreshTokenKey"))
```

**Example Code**
> For Codable
```
import BHAPIManager

class LoginServices {
    
    static func callLogin(onSuccess: @escaping (LoginResponse) -> (), onError: @escaping (String) -> ()) {
        API.Client.request(router: LoginRouter.login(params: LoginRequest(email: "barotbhavik23@yopmail.com", password: "Test@123")))
            .basicAuth(enabled: false)
            .response(forModel: LoginResponse.self) { res in
            onSuccess(res)
        } onError: { error in
            onError(error?.localizedDescription ?? "")
        }
    }
}
```
> For JSON

```
import BHAPIManager

class LoginServices {
    
    static func callLogin(onSuccess: @escaping ([String: Any]) -> (), onError: @escaping (String) -> ()) {
        API.Client.request(router: LoginRouter.login(params: LoginRequest(email: "barotbhavik23@yopmail.com", password: "Test@123")))
            .basicAuth(enabled: false)
            .responseJSON { res in
            onSuccess(res)
        } onError: { error in
            onError(error?.localizedDescription ?? "")
        }
    }
}
```

> 🚧: Try default top level structure to get smooth data parsing and validated response.
```
success: Bool?
status: Bool?
statusCode: Int?
message: String?
result: T? // Will be your Response Model Object
data: T? // Will be your Response Model Object
```

Author
---
Bhavik Barot , email at: barotbhavik23@gmail.com
