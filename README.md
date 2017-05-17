
# Upstox API Nodejs client
The official Javascript node client for communicating with the Upstox APIs
Upstox Node Js Library provides an easy to use wrapper over the HTTPs APIs. The HTTP calls have been converted to methods and their JSON responses.
Moreover we provide websocket connection to get live updates from Upstox APIs.

## Installation

This module is installed via npm:

	 npm install --save Upstox

## Documentation
   [Upstox API documentation](https://upstox.com/developer/api/v1/docs/)

Getting started with API
------------------------

### Authentication with Oauth

     To start using upstox services -
     ------------------------------
     Require Upstox - 
         var Upstox = require("Upstox");
         
     Craete an upstox object by passing apiKey as a parameter. (Note: apiKey is required field)
         var upstox = new Upstox("your apiKey");

     1. Get authentication url with getLoginUri method. Params required are - apiKey, redirect_uri(provided while creating the app on developer console of Upstox), response_type
        var loginUrl = upstox.getLoginUri(testdata.accessToken.redirect_uri, "code");
     
     2. Get authenticated with your uccid and password with Upstox.
        Upstox Login screen will be popup up with which you need to get logged in.
        
     3. On completion of authentication user will be redirected to the redirect_uri with code added in query parameter.
     
     4. Get the accessToken required for getting authenticated for all subsequent API calls.
         getAccessToken method requires following- 
            var params: {
                "apiSecret" : "your_apiSecret",
                "code" : "your_code_generated_in login",
                "grant_type" : "authorization_code",
                "redirect_uri" : "your_redirect_uri"
            };
            
            var accessToken;
            
            upstox.getAccessToken(params)
                .then(function(response) {
                  accessToken = response.access_token;
                })
                .catch(function(err) {
                    // handle error 
                });
                
     5. Set access token by invoking method called setAccessToken(your_access_token); // pass the accessToken generated in response with getAccessToken.
        
        upstox.setToken(accessToken);
        
     
    
### Examples

     Subsequent services can be called as shown in below
    
        // Fetch holdings.
        // You can have other api calls here.

        // Get Holdings
        upstox.getHoldings()
            .then(function(response) {
                // You got user's holding details.
            })
            .catch(function(err) {
            // Something went wrong.
        });

        // GetProfile
        upstox.getProfile()
            .then(function(response) {
                // You got user's holding details.
            })
            .catch(function(err) {
            // Something went wrong.
        });

        // PlaceOrder
        var orderObject = {
                transaction_type:"b",
                exchange:"NSE_EQ",
                symbol: "RELIANCE",
                quantity: 1,
                order_type:"m"
             };
             
        upstox.placeOrder(orderObject)
            .then(function(response) {
                // You got user's holding details.
            })
            .catch(function(err) {
            // Something went wrong.
        });


## Websocket
 
        Every service returns a promise which will either be resolved or rejected.
         *
         *  Websocket services are available for following events -[live data, position updates, tradeUpdates, liveFeeds]
         *
         *  To get an active socket connection use connectSocket Method.
      
        // Socket
        upstox.connectSocket()
            .then(function(){
                // Socket Connection successfull
                // Now you can setup listeners
                upstox.on("orderUpdate", function(message) {
                    //message for order updates
                });
                upstox.on("positionUpdate", function(message) {
                    //message for position conversion
                });
                upstox.on("tradeUpdate", function(message) {
                    //message for trade updates
                });
                upstox.on("liveFeed", function(message) {
                    //message for live feed
                });
                upstox.on("disconnected", function(message) {
                    //listener after socket connection is disconnected
                });
                upstox.on("error", function(error) {
                    //error listener
                });
                //You can call upstox.closeSocket() to disconnect
            }).catch(function(err) {
                // Something went wrong.
            })
            
## Test
    npm test // Please add the oauth token details in testData.json
   
## Licence