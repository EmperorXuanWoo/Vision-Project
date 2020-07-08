#  전주비전대 project 1

## Install DHT11 sensor
```
git clone https://github.com/adafruit/Adafruit_Python_DHT.git
cd Adafruit_Python_DHT
sudo python setup.py install
cd examples
```
  - Run
  ```
  python AdafruitDHT.py 11 4
  ```
  
# InfluxDB Installation
  # 1. Repository dml  GPG key 구하기
  ```
  curl -sL https://repos.influxdata.com/influsdb.key | sudo apt-key add -
  ```
  # 2. Repository를 더하기
  ```
  echo "deb https://repos.influxdata.com/debian stretch stable" | sudo tee
  ```
  # 3. 프로그램 설치
