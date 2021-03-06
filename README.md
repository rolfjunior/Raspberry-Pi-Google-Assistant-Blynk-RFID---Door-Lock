# Raspberry-Pi-Google-Assistant-Blynk-RFID---Door-Lock
Control a door lock using Blynk app, Google Assistant, RFID  tags or buttons.
1. Able to Read RFID cards for Authentication and will unlock and lock the Door
2. Able to Read Button inputs for unlocking and locking the Door
3. Use blynk app to control the door lock, control blynk app using google assistant
4. Blink leds to show door status

#HOW TO INSTALL

Check if step 1 is necessary since it looks like the code uses only the pirc522 to read RFID tags.
This step is for reading NFC tags, but I couldn't make it work and therefore is not necessary if you don't need to use NFC to unlock or lock the door.

13.Edit /etc/rc.local. This will make VNC server to be prepared at boot for remote access

	13.1 - sudo nano /etc/rc.local
	
	13.2 - add the line at the end of the file	su - pi -c '/usr/bin/vncserver :1' &


1.  Folow the steps on this link http://www.instructables.com/id/Raspberry-Pi-3-Model-B-MIFARE-RC522-RFID-Tag-Readi/

	1.1 - raspi-config  ... Use the interactive menu to enable the SPI Interface
	
	1.2 - sudo reboot
	
	1.3 - sudo nano /boot/config.txt   ...check to see that the SPI Interface is enabled, Try to find a line that says: dtparam=spi=on If you see the above line then SPI is enabled
	
	1.4 - lsmod | grep spi   ...check to see that the spi_bcm2835 module is loaded
	
	1.1 - sudo apt-get install python2.7-dev   ...Install python2.7-dev:
	
	1.2 - git clone https://github.com/lthiery/SPI-Py.git
	
	1.3 - cd SPI-Py
	
	1.4 - sudo python setup.py install
	
	1.5 - cd
	
	1.6 - git clone https://github.com/mxgxw/MFRC522-python.git
	
	1.7 - cd MFRC522-python
	
	1.8 - python Read.py

2. I couldn't make NFC reading work to unlock or lock the door. But the step is still necessary. Install pirc522 from this link https://github.com/ondryaso/pi-rc522

	2.1 - git clone https://github.com/ondryaso/pi-rc522.git
	
	2.2 - cd pi-rc522
	
	2.3 - sudo python setup.py install
	
	2.4 - cd

3.Clone the code from git and copy all the files in your raspberry pi under /home/pi

	3.1 - sudo git clone https://github.com/rolfjunior/Raspberry-Pi-Door-Lock
  
4.A folder will be created with the files inside of it move the files to /home/pi/Raspberry-Pi-Door-Lock
Using file manager do copy and paste or

	4.1 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/locker.py /home/pi/
	
	4.2 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/start_lock.sh /home/pi/
	
	4.3 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/card_data.json /home/pi/
	
	4.4 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/door_lock.py /home/pi/
	
	4.5 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/servolock.py /home/pi/
	
	4.6 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/blynk.py /home/pi/
	
	4.7 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/lockstate.pickle /home/pi/
	
	4.8 - sudo cp /home/pi//Raspberry-Pi-Door-Lock/lockstate.pickle /createpickle.py
	
	4.8 - sudo rm -r /home/pi//Raspberry-Pi-Door-Lock

5.Delete the files on the original folder

	5.1 - sudo rm /home/pi//Raspberry-Pi-Door-Lock/*   ...to delete all
	
	5.2 - sudo rm /home/pi//Raspberry-Pi-Door-Lock/door_lock.py   ...to delete one file 

6.Give Executable Access to locker.py

	6.1 - sudo chmod -x /home/pi/locker.py or sudo chmod 777 /home/pi/locker.py
	
7.Give Executable Access to lockstate.pickle

	6.1 - sudo chmod -x /home/pi/lockstate.pickle or sudo chmod 777 /home/pi/lockstate.pickle
	
	
7. Install screen to run program in a diferent screen

	7.1 - sudo apt-get install screen
	
	7.2 - use screen -list to see a list of runing screens

7. Use the command line "python door_lock.py L" to Move the Servo to Lock Position for the first time For installation

	7.1 - python door_lock.py L

8.Start the program

	8.1 - sudo python locker.py

9.Scan the RFID key fobs and Edit the key data in card_data.json, Whenever you scan a new key Fob it will print some card data such as
[82,101,194,16,220] copy that from the output screen and update the card_data.json dictionary value or add a new value as you see fit.

10.Edit crontab. This is to start up the lock program during raspberry boot

	10.1 - crontab -e
	
	10.2 - add the line at the end of the file without #    	@reboot /bin/sh /home/pi/start_lock.sh

11.Install Blynk, follow instructions from this link: http://help.blynk.cc/how-to-connect-different-hardware-with-blynk/raspberry-pi/how-to-install-nodejs-library-on-linux

	11.1 - sudo npm install blynk-library -g
	
	11.2 - sudo npm install onoff -g
	
	For the raspberry pi zero w another methode for installing node js must be used
	
	11.1 - follow instructions on  http://help.blynk.cc/how-to-connect-different-hardware-with-blynk/raspberry-pi/how-to-install-nodejs-library-on-linux

	11.4 - sudo pip install blynk-library-python
	
12.Install Blynk app on the smartphone

	12.1 - Create new project with one virtual button
	
	12.2 - Update the blynk.py file with the blynk autentication code  sudo nano blynk.py
	
14.Reboot the Pi and the software will start working.

	14.1 - sudo reboot
