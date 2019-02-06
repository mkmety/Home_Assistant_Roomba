# Home_Assistant_Roomba
Using HA to send iOS notifications to run when away.

I decided I wanted Home Assistant to send a notification to my iOS device whenever I was 'Away'. For me, this meant whenever my phone was not connected to my WiFi, home assistant would show me as 'Away'. 

After 10 minutes of being away, I would get a custom iOS notification from Home Assistant asking me if I'd like to start Roomba.

Below are the steps and excerpts from my config files that allowed this to work for me. 

1. Home assistant needs to be able to see if you are Home or Away. I use nmap to have my Home/Away status be based on whether or not I am connected to my home WiFi. Typically, if I am home, I will be connected to my WiFi. If I leave I get disconnected and nmap has Home Assistant mark me as Away. This works pretty well for the most part. You'll need to add nmap to your configuration.yaml file (See device_tracker in configuration.yaml).

I set a static IP for my phone through my router. This way, my phone gets the same IP each time I connect/disconnect. I have home_interval set up for 10 minutes. This device will only be scanned every 10 minutes to help relieve traffic on my network and preserve battery life on my device. This is also good because if I happen to turn my phone on/off, take out the garbage, or disconnect from my WiFi for less than 10 minutes then I won't be marked as Away. This cuts down on fale positive notifications later on. Adjust as you see fit. 

1. You first need to add roomba to your configuration.yaml file (see configuraiton.yaml).
2. The username and password can be retreived using the following library: https://github.com/koalazak/dorita980#how-to-get-your-usernameblid-and-password
3. You then need to add the ios portion to your configuration.yaml file (see configuraiton.yaml).
