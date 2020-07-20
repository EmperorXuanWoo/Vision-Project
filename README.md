#  전주비전대 project 1

  ## Install DHT11 sensor
  ```
  git clone https://github.com/adafruit/Adafruit_Python_DHT.git
  cd Adafruit_Python_DHT
  sudo python setup.py install
  cd examples
  ```
  - run
  ```
  python AdafruitDHT.py 11 4
  ```
  
  # InfluxDB Installation

  ## 1. Repository dml  GPG key 구하기
   ```
   curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -
   ```
  
  ## 2. Repository를 더하기
  ```
  echo "deb https://repos.influxdata.com/debian stretch stable" | sudo tee /etc/apt/sources.list.d/influx.list
  ```
  
  ## 3. 프로그램 설치
  ```
  sudo apt update
  sudo apt install influxdb
  ```
  
  ## 4. 프로그램 실행
  ```
  sudo service influxdb start
  ```
  
  ## 5. 데이터베이스 만들기
  ```
  >create database <데이터베이스이름>
  ```
  ```
  확인 : show databases
  ```
  
  # Grafana Installation
  
  ## 1. Repository dml  GPG key 구하기
  ```
  curl https://bintray.com/user/downloadSubjectPublicKey?username=bintray | sudo apt-key add -
  ```
  
  ## 2. Repository를 더하기
  ```
  echo "deb https://dl.bintray.com/fg2it/deb stretch main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
  ```
  
  ## 3.프로그램 설치
  ```
  sudo apt update
  sudo apt install grafana
  ```
  
  ## 4. 프로그램 실행
  ```
  sudo service grafana-server start
  ```
  ## influxdb import with python
  ```
  sudo pip install influxdb
  ```
  ## gpio pin map
  ```
  cd /tmp
  wget https://project-downloads.drogon.net/wiringpi-latest.deb
  sudo dpkg -i wiringpi-latest.deb
  ```
  
  ### GIT HUB 사용방법
  - Repositoty down load
  ``` 
  git clone https://github.com/<username>/<repository name>
  ```
  - GIT HUB Uploadpwd
  ```
  git add .
  git commit -m <주석?이름?(자기맘대로)>
  git config --global user.email "홍길동@gmail.com"
  git config --global user.name "이름"
  git push
  ```
  ### GIT HUB  기본명령어
  ```
  - ls : 현재 경로 파일 리스트
  - cd : 디렉토리 변경
  - mkdir : 디렉토리 생성
  - pwd : 현재 디렉토리
             .
             .
             .
  ```
  ### Vim Editor Setting
  ```
  set nu        // Line number
  set cindent   // C language indent
  set ts=4      // tab size 4
  set softtabstop=4
  set bg=dark
  set expandtab
  let python_version_2 = 1
  let python_highght_all = 1
  filetype indent plugin on
  
  if has("syntax")      // syntax on
    syntax on
  endif
  ```
  ### Python
  ```
  #!/usr/bin/python
  
  import time
  import RPi.GPIO as GPIO
  
  print GPIO.VERSION
  GPIO.setmode(GPIO_BCM)
  GPIO_setup(4,GPIO.IN)
  
  def interrupt_fired(channel):
      print("interrupt Fired")
      print(channel)
  GPIO.add_event_detect(4,GPIO.FALLING, callback=inerrupt_fired)
  
  while(True):
      time.sleep(1)   
      print("timer fired")
  ```
  ### Python pir.py
  ```
  #!/usr/bin/python

  import time
  import RPi.GPIO as GPIO
  import requests, json
  from influxdb import InfluxDBClient as influxdb

  GPIO.setmode(GPIO.BCM)
  GPIO.setup(4, GPIO.IN)

  def interrupt_fired(channel):
      print("interrupt Fired")
      a = 5
      data = [{
          'measurement' : 'pir',        
          'tags':{
              'VisionUni' : '2410',
          },
          'fields':{
              'pir' : a,
          }
      }]
      client = None
      try:
          client = influxdb('localhost',8086,'root','root','pir')
      except Exception as e:
          print "Exception" + str(e)
      if client is not None:
          try:
              client.write_points(data)
          except Exception as e:
              print "Exception write " + str(e)
          finally:
              client.close()
      print(a)
  GPIO.add_event_detect(4, GPIO.FALLING, callback=interrupt_fired)

  while(True):
      time.sleep(1)
      a = 1
      data = [{
          'measurement' : 'pir',        
          'tags':{
              'VisionUni' : '2410',
          },
          'fields':{
              'pir' : a,
          }
      }]
      client = None
      try:
          client = influxdb('localhost',8086,'root','root','pir')
      except Exception as e:
          print "Exception" + str(e)
      if client is not None:
          try:
              client.write_points(data)
          except Exception as e:
              print "Exception write " + str(e)
          finally:
              client.close()
      print("running influxdb OK")
  ```
  ### Python co2.py
  ```
  #!/usr/bin/python
  
  import sys, serial, time
  
  comm = '/dev/ttyAMA0'
  baudrate = 38400
  
  device = serial.Serial(comm,baudrate, timeout = 5)
  print(device)
  
  while(True):
      try:
          rcvBuf = bytearray()
          device.reset_input_buffer()
          rcvBuf = device.read_untill(size=12)
          print rcvBuf
      except Exception as e:
          print("exception read") + str(e)
          
      time.sleep(5)
  ```
  
  ### 라즈베리파이 설정 변경 -serial 설정
  ```
  sudo raspi-config
  ```
  
  ### Pyton 파일 복사하기
  ```
  cp 복사할파일 복사할위치
  cp 복사할파일 /home/pi/<directory>
  ./ --> 현재위치
  ```
  ### 이산화탄소 Sensor
  ```
  vim co2.py                                  | co2.py 파일 생성
  sudo vim /boot/config.txt
  dtoverlay=pi3-disable-bt                    | /boot/config.txt 파일 가장 하단에 작성
  sudo systemctl disable huiuart              | 블루투스 제거( 사용안함 )
  sudo reboot                                 | 리붓
  ```
  ###  라즈베리파이 Telegram API 설치
  ```
  pip3 install python-telegram-bot --upgrade
  git clone https://github.com/python-telegram-bot/python-telegram-bot --recursiv
  
  ```
