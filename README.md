# My-Accountant-Broker-Bot
Set up a learning robot with a "paper" stock trading account that makes decisions based on API data.

# Goals

1.[Setting Up Your Pi](#setting-up-the-pi) 

2.[Setting Up Connection](#setting-up-a-connection)

3.[Installing Bot](#installing-the-bot)

4.[Getting Key(s)](#getting-your-keys)

DISCLAIMER:
**_This is for educational purposes ONLY, we have set this up with a "paper wallet" ACCOUNT. Don't be fucking retarded._**

We wanted to focus on the easiest way to make money while pursuing computer science. IF computer science is the answer to everything THEN the problem of emotional trading can be solved with computer science, we just need to set the rules, but It looks like [Github has some rules set up already.](https://github.com/roguesupport-scott/Machine-Learning-Stock-Trading-Bot)

I don't actually know Python, I just know how to find the parts I need and to put them together, I have learned a lot about **file** structures and how to edit individual **files**. Learning computer science has been the fastest feedback mechanism I've ever encountered in my life. All the heavy lifting has been done, the sandbox is already built!

We will set this `robot agent` up on a Raspberry Pi 4, we'll need at least 16 gb flash memory and the ability to hook the Pi4 up to a screen and keyboard.

---
#### Setting Up The Pi
---
First we will install the [Raspberry Pi Imager](https://github.com/raspberrypi/rpi-imager)
and then we will install an operating system onto the PI. You can also visit the [official site](https://www.raspberrypi.com/software/).


![Pasted image 20221006145533](https://user-images.githubusercontent.com/94795740/194461935-4b61aeb5-92da-4ce6-9ab9-b6832fa97aa5.png)


Format the disk you are using, in our case we will use a 32 gb micro SD we have lying around. Our version of the Imager is 1.7.3 as of this commit.


![Pasted image 20221006150125](https://user-images.githubusercontent.com/94795740/194462157-07833825-b5ab-4037-be99-86fc9f9208b1.png)


We should edit our settings here to make everything easy:
1. Name your machine
2. Enable SSH 
3. Set a username and password
4. configure you wireless LAN if you want, but we prefer a hard connection through ethernet.


![Pasted image 20221006164608](https://user-images.githubusercontent.com/94795740/194462223-af743194-8009-426d-8479-d319e533e140.png)


Choose the storage and operating system (Ubuntu 20.04), and **write** to the disk.


![Pasted image 20221006150217](https://user-images.githubusercontent.com/94795740/194462280-849c274a-6746-4528-b33e-f6db00badf70.png)


After you have completed **writing** the disk, install it into the raspberry Pi and [SSH in.](https://itsfoss.com/ssh-into-raspberry/)

---
#### Setting Up A Connection
---
Here is the point where we need to decide how we're going to obtain the IP address information for the Pi, our Pi has already been assigned a static IP addresses from the [firewall/gateway](https://www.ipfire.org/) based on it's [MAC address](https://en.wikipedia.org/wiki/MAC_address).

Perhaps you have a wireless connection instantly through your home router and can find the IP address there if you enabled wireless.

Maybe you have a Pi 400 and are doing everything directly on the machine from this point on. We will not, we will need to find the IP address of the device in order to proceed with SSH.

If you're looking for the IP through a keyboard connection to the actual Pi, then you can log into the machine and type:

>$ ifconfig

Ubuntu will tell you how to install this as it should not be installed yet. It's best practice to update Linux before adding new programs.

>$ sudo apt-get update && sudo apt-get upgrade -y

After installing net-tools you should be able to use `ifconfig` and you should see the connection you've established to the network, I found this example below.


![Pasted image 20221006161704](https://user-images.githubusercontent.com/94795740/194462313-ef5138b5-afcf-4ca9-92ac-08e425383f2d.png)


Now we can go to our terminal on our regular workstation computer and SSH into the device.

>ssh pi@192.168.0.15

This will allow us to copy/paste everything else directly to the device without an SSH connection. You could try doing everything from the Pi directly but I don't recommend it.

---
#### Installing The Bot
---

Now we should have command line access to the Pi from our workstation.

First make sure the following are installed in linux.

>$ sudo apt install python3-pip -y

>$ sudo apt-get install -y python3-websockets

>$ pip3 install alpaca-trade-api

At this point you should get a warning that says the "location" is not on [PATH](https://linuxconfig.org/linux-path-environment-variable). We can add the path by taking the "location" and adding it with the following line:

>$ export PATH=/home/pi/.local/bin:$PATH

"pi" should be your username, but it will be evident in the warning, what you had set up earlier. Next we will install [News API](https://newsapi.org/docs/client-libraries/python).

>$ pip install newsapi-python

Then we will install [Textblob](https://textblob.readthedocs.io/en/dev/api_reference.html).

>$ pip install textblob

Then we install [Scikit-learn](https://scikit-learn.org/stable/)

>$ pip install scikit-learn

Then we need to make a blank text file with nano for the program to reference and/or **print** to.

>$ sudo nano target_price_list.txt

**Note**: I use a blank space as the only character in blank documents so that the document actually saves.

We will uses nano, and paste the [following code](https://github.com/roguesupport-scott/Machine-Learning-Stock-Trading-Bot/blob/master/__main__.py) from the \_\_main__.py file. We are creating this **file** manually because this is the best way to show you what's in a **file**. I've not included the text here as that link has a `copy` button for you.

> $ sudo nano \_\_main__.py

or use the following command to just git it done. This will automate all of the above.

>$ wget https://raw.githubusercontent.com/roguesupport-scott/Machine-Learning-Stock-Trading-Bot/master/__main__.py


![Pasted image 20221006201007](https://user-images.githubusercontent.com/94795740/194462741-ace6cf14-88f9-4858-9e25-f6c4bc38a2ae.png)


Now we will close nano and enter `ctrl+x` and `Y` to save the **file**.

We also need to create two more files, The [Forecast Library](https://github.com/roguesupport-scott/Machine-Learning-Stock-Trading-Bot/blob/master/forecast_library.py),

>$ sudo nano forecast_library.py

git it done.

>$ wget https://raw.githubusercontent.com/roguesupport-scott/Machine-Learning-Stock-Trading-Bot/master/forecast_library.py


![Pasted image 20221006201646](https://user-images.githubusercontent.com/94795740/194462775-f9d8fed7-8af3-46f0-930f-829e82702182.png)


and the [Config File](https://github.com/roguesupport-scott/Machine-Learning-Stock-Trading-Bot/blob/master/config.py).

>$ sudo nano config.py

git it done again.

>$ wget https://raw.githubusercontent.com/roguesupport-scott/Machine-Learning-Stock-Trading-Bot/master/config.py

![image](https://user-images.githubusercontent.com/94795740/194684453-6dc4b007-75cd-4081-befb-ad198bdfc06e.png)

---
#### Getting Your Keys
---

We will have to get our API keys from the sources here:

1.[Alpha Vantage](https://www.alphavantage.co/)(ALPHAV_API_KEY).

2.[News API](https://newsapi.org/) (api_key).

3.[Alpaca](https://app.alpaca.markets/brokerage/new-account) (ALPACA_API_KEY).

4.[Alpaca](https://app.alpaca.markets/brokerage/new-account) (ALPACA_SECRET_KEY).

Its important to keep the keys private, here we have provided dummy example keys.

This was probably one of the simplest steps, just follow along above and make sure that the keys are wrapped in `'`quotes`'` after the `=` sign, or the program will not run.

Open the config.py file using nano. Add the API keys you collected to the contents of the file manually.

>$ sudo nano config.py

Run the program

>$ python3 \_\_main__.py

At this point I get a warning on the Pi, "PermissionError: [Errno 13] Permission denied: 'target_price_list.txt'". I just worked around this with chmod.

>$ sudo chmod a+r+w target_price_list.txt

Run it again and this is what we get.

![image](https://user-images.githubusercontent.com/94795740/194682215-d8a3f2b7-8881-4b1a-9a40-35596853690b.png)

Now go check your transaction history on your paper wallet account!



#ScottDuncanIsAlwaysRight

#ComputerScienceIsTheAnswerToEverything


