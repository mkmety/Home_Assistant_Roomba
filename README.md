# Home_Assistant_Roomba
Using HA to send iOS notifications to run when away.

I decided I wanted Home Assistant to send a notification to my iOS device whenever I was 'Away'. For me, this meant whenever my phone was not connected to my WiFi, home assistant would show me as 'Away'. 

After 10 minutes of being away, I would get a custom iOS notification from Home Assistant asking me if I'd like to start Roomba.

Below are the steps and excerpts from my config files that allowed this to work for me. 

1. You first need to add roomba to your configuration.yaml file (see configuraiton.yaml).
2. The username and password can be retreived using the following library: https://github.com/koalazak/dorita980#how-to-get-your-usernameblid-and-password
3. You then need to add the ios portion to your configuration.yaml file (see configuraiton.yaml).
