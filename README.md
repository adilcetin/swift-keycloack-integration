# swift-keycloack-integration

Pod File

    target 'Your Project' do

    use_frameworks!

    # Pods
    pod 'Alamofire'
    pod 'Alamofire-Synchronous'
    
    
# Login
    func login(username: String, password: String) -> OpenIDAuthResult! {
        
        let url = http://" + YourServerIp + ":" + YourServerPort + "/auth/realms/" + YourRealmName + "/protocol/openid-connect/token"
         
        let parameters : [String: String] =
        [
            "grant_type": "password",
            "client_secret": "*** YOUR SERVER SECRET ***",
            "client_id": "*** YOUR CLIENT ID ***",
            "username": username,
            "password": password,
            "scope": "offline_access"
        ]
          
        let response = Alamofire.request(url, method: .post, parameters: parameters).responseData()
        
        switch response.result {
            case let .success(value):
                do {
                    let openIdAuthResult = try JSONDecoder().decode(OpenIDAuthResult.self, from: value)
                    return openIdAuthResult
                }
                catch {
                    print("Error info: \(error)")
                    
                }

            case let .failure(error):
                print(error)
        }
        return nil
    }
    
    
 # Refresh your token
     func refreshToken() -> OpenIDAuthResult! {
        let url = http://" + YourServerIp + ":" + YourServerPort + "/auth/realms/" + YourRealmName + "/protocol/openid-connect/token"
         
        let parameters : [String: String] =
        [
            "grant_type": "refresh_token",
            "client_secret": "*** YOUR SERVER SECRET ***",
            "client_id": "*** YOUR CLIENT ID ***",
            "refresh_token": "*** YOUR REFRESH TOKEN ***"
        ]
          
        let response = Alamofire.request(url, method: .post, parameters: parameters).responseData()
        
        switch response.result {
            case let .success(value):
                do {
                    let openIdAuthResult = try JSONDecoder().decode(OpenIDAuthResult.self, from: value)
                    return openIdAuthResult
                }
                catch {
                    print("Error info: \(error)")
                    
                }

            case let .failure(error):
                print(error)
        }
        return nil
    }
    
    
# Logout
     func logout() -> Bool {
        let url = http://" + YourServerIp + ":" + YourServerPort + "/auth/realms/" + YourRealmName + "/protocol/openid-connect/logout"
         
        let parameters : [String: String] =
        [
           "client_secret": "*** YOUR SERVER SECRET ***",
            "client_id": "*** YOUR CLIENT ID ***",
            "refresh_token": "*** YOUR REFRESH TOKEN ***"
        ]
          
        let response = Alamofire.request(url, method: .post, parameters: parameters).responseData()
        return response.result.isSuccess
    }
