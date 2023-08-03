---
title:  "Data Logger - Part 2 - Storage"
excerpt: "This is the second in a series of posts to build a simple data logger system for temperature and relative humidity. Part 2 will send data to an Amazon Timestream database."
date:   2021-11-15 00:00:00
categories:
  - IoT
  - DATA SCIENCE
tags:
  - Python
  - Raspberry Pi
  - AWS
  - AWS Timestream
---

**Introduction**

This is the second in a series of posts to build a simple data logger system for temperature and relative humidity. 

Posts in the series:

* [Part 1 - Hardware - build a data logger for recording temperature and humidity using a Raspberry Pi]({% post_url 2020-10-31-data-logger-part-1-hardware %})
* Part 2 - Storage - send data to an Amazon Timestream database 
* [Part 3 - Visualization - viewing your data in a graphical form]({% post_url 2021-11-15-data-logger-part-3-visualization %})

**Create Timeseries Database**

[Amazon Timestream](https://aws.amazon.com/timestream/) is a fast, scalable, and serverless time series database service for IoT and operational applications that makes it easy to store and analyze trillions of events per day.

To create, query and delete the database we will use the [AWS Data Wrangler](https://aws-data-wrangler.readthedocs.io/en/stable/index.html) an AWS Professional Service open source python initiative that extends the power of Pandas library to AWS connecting DataFrames and AWS data related services.

{% highlight python %}
import awswrangler as wr

DATABASE_NAME = "DataLoggerDB"
TABLE_NAME = "IoT"

wr.timestream.create_database(DATABASE_NAME)
wr.timestream.create_table(DATABASE_NAME, TABLE_NAME, memory_retention_hours=1, magnetic_retention_days=30)
{% endhighlight %}

**Paspberry Pi Configuration**

Raspberry Pi OS 

{% highlight shell %}
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Raspbian
Description:	Raspbian GNU/Linux 11 (bullseye)
Release:	11
Codename:	bullseye
{% endhighlight %}

Python

{% highlight shell %}
$ python3 --version
Python 3.9.2
{% endhighlight %}

Install Adafruit Python DHT

{% highlight shell %}
 $ sudo apt-get update

 $ sudo apt-get install python3-pip

 $ sudo python3 -m pip install --upgrade pip setuptools wheel

 $ sudo pip3 install Adafruit_DHT
{% endhighlight %}

AWS CLI

{% highlight shell %}
 $ pip3 install awscli --upgrade --user

 $ export PATH=/home/pi/.local/bin:$PATH

 $ aws --version
 aws-cli/1.22.5 Python/3.9.2 Linux/5.10.63+ botocore/1.23.5

 $ aws configure
 AWS Access Key ID [None]: ********************
 AWS Secret Access Key [None]: ****************************************
 Default region name [None]: eu-west-1
 Default output format [None]: json
{% endhighlight %}

**Update Data Logger Script**

The example Python script created in part 1 is updated to add the AWS Timestream logging.

Import the boto3 AWS library.
{% highlight python %}
import boto3

from botocore.config import Config
{% endhighlight %}

Add constants for the Timestream database and table names.

{% highlight python %}
DATABASE_NAME = "DataLoggerDB"
TABLE_NAME = "IoT"
{% endhighlight %}

Write the temperature and relative humidity values.

{% highlight python %}
# Write to Timestream database
write_to_timestream ('Temperature', 'DegC', temperature)
write_to_timestream ('Relative Humidity', '%', humidity)
{% endhighlight %}

The python function to write rhe records to Timestream.

{% highlight python %}
def write_to_timestream(measurement, unit, value):

    # Create AWS session
    session = boto3.Session()

    client = session.client('timestream-write', config=Config(read_timeout=20, max_pool_connections=5000,
                                                                    retries={'max_attempts': 10}))

    # Build record to send to Timestream
    dimensions = [
        {'Name': 'SensorId', 'Value': SENSOR_ID},
        {'Name': 'Location', 'Value': SENSOR_LOCATION},
        {'Name': 'Unit', 'Value': unit}
    ]

    record = {
        'Dimensions': dimensions,
        'MeasureName': measurement,
        'MeasureValue': str(value),
        'MeasureValueType': 'DOUBLE',
        'Time': str(int(round(time.time() * 1000)))
    }

    records = [record]

    # Write record to Timestream and log errors
    try:
        client.write_records(DatabaseName=DATABASE_NAME, TableName=TABLE_NAME,
                                            Records=records, CommonAttributes={})

    except client.exceptions.RejectedRecordsException as err:
        print("RejectedRecords: ", err)
        for rr in err.response["RejectedRecords"]:
            print("Rejected Index " + str(rr["RecordIndex"]) + ": " + rr["Reason"])
        print("Other records were written successfully. ")

    except Exception as err:
        print("Error:", err)
{% endhighlight %}

**Run Data Logger Script**

{% highlight shell %}
$ python3 data_logger.py
Logging sensor measurements to data.txt every 60 seconds.
Press Ctrl-C to quit.
Timestamp                   SensorId  Location  Measurement        Value  Unit
---------                   --------  --------  -----------        -----  ----
2021-11-15T11:59:59.094528  001       Office    Temperature        23.0   DegC
2021-11-15T11:59:59.094528  001       Office    Relative Humidity  65.0   %
2021-11-15T12:01:17.322727  001       Office    Temperature        23.0   DegC
2021-11-15T12:01:17.322727  001       Office    Relative Humidity  65.0   %
2021-11-15T12:02:28.671775  001       Office    Temperature        23.0   DegC
2021-11-15T12:02:28.671775  001       Office    Relative Humidity  66.0   %
{% endhighlight %}

**Query AWS Timestream Database**

{% highlight python %}
import awswrangler as wr

DATABASE_NAME = "DataLoggerDB"
TABLE_NAME = "IoT"

SELECT_SQL = f"SELECT * FROM {DATABASE_NAME}.{TABLE_NAME} ORDER BY time DESC LIMIT 10"

df = wr.timestream.query(SELECT_SQL)

print(df)
{% endhighlight %}

{% highlight shell %}
$ python3 timestream_query.py

  SensorId  Unit Location       measure_name                    time  measure_value::double
0      001     %   Office  Relative Humidity 2021-11-15 13:16:29.992                   60.0
1      001  DegC   Office        Temperature 2021-11-15 13:16:25.109                   24.0
2      001     %   Office  Relative Humidity 2021-11-15 13:15:14.630                   58.0
3      001  DegC   Office        Temperature 2021-11-15 13:15:09.785                   24.0
4      001     %   Office  Relative Humidity 2021-11-15 13:14:02.037                   58.0
5      001  DegC   Office        Temperature 2021-11-15 13:13:56.315                   24.0
6      001     %   Office  Relative Humidity 2021-11-15 13:12:50.947                   61.0
7      001  DegC   Office        Temperature 2021-11-15 13:12:45.702                   24.0
8      001     %   Office  Relative Humidity 2021-11-15 13:11:40.339                   61.0
9      001  DegC   Office        Temperature 2021-11-15 13:11:35.530                   24.0
{% endhighlight %}

**Delete AWS Timestream Database**

{% highlight python %}
import awswrangler as wr

DATABASE_NAME = "DataLoggerDB"
TABLE_NAME = "IoT"

wr.timestream.delete_table(DATABASE_NAME, TABLE_NAME)
wr.timestream.delete_database(DATABASE_NAME)
{% endhighlight %}

**Summary**

This post describes now to take Temperature and Relative Humidity readings from the sensor attached to the Raspberry Pi and write those readings to an Amazon Timestream database.

The code for this post is available on [GitHub](https://github.com/jonathanoneill/data-logger-blog-post/tree/data-logger-part-2-storage).
