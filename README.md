**deviceparser.py** is a tool that I wrote to help a user parse the rich information available the **devices_data.txt** file. This file contains the specs of all devices from [GSMArena](https://www.gsmarena.com/) stored in the JSON format. I created this file by building a webcrawler from scratch in Python, using BeautifulSoup, which programmatically crawled across 9500+ webpages to assemble this information. You can learn more how to build you own web crawler using the first tutorial in my **Truly End-to-End Machine Learning** series, linked [here](https://github.com/vigvisw/end2endml). 


### Instructions For Using deviceparser.py
All full walkthrough of this process can be found in deviceparser_example.ipynb.

1. Download *deviceparser.py* and move it to [Python's working directory](https://stackoverflow.com/questions/17359698/how-to-get-the-current-working-directory-using-python-3/17361545).
2. Import the module into your IDE.
3. At this point, you can define you own parsing functions which must comply with **Rules For Writing Parsing Functions For Devices Data**.
4. Expose the **ParsingFunctions** class to user defined functions by passing a list of functions to the **ParsingFunctions.add_new_parsers()** method.
5. Load the **devices_data.txt** file by passing the file path to **Device.read_devices_json()**. This loads in the JSON file as a Python dictionary.
6. A list of device objects can be created for all the makers by providing the **Device.create_devices_from_data()** method with the dictionary obtained in step 5.
7. To make it easy to manipulate the data once it has been parsed, the **Device.create_df()** method has been defined, which creates a pandas DataFrame from *devices_data*. As an argument, this method takes in the dict object which was loaded in step 5 and applies **Device.create_devices_from_data()**. An internal dictionary called **all_features_dict** is used to keep track of all the features collected from all the devices. For example, flagship devices released in 2018 and 2019 tend to have three main cameras, which is denoted by the feature *main_camera_triple*. This method is written such that a  device feature listed in the keys of **all_features_dict** is given a value and np.NaN, otherwise.


**Rules For Writing Parsing Functions For Devices Data**
1. Currently parsing functions can only be defined for **specs**. This includes everything we collected on a device's page. I have kept is this way because, these are things which I conisider a device's feature. You can modify the three classes to parse device information such as **device_name**, **maker_name** etc, if you choose.
2. The parsing function **must** follow the name format 'parse_' + spec_name. For example, if you are trying to parse the feature **platform_os** as seen in the columns of the *Skeleton Dataset*, the parsing function which you define must be be named **parse_platform_os**.
3. The return value of the parsing function **must** be a dictionary of the format **{*new_feature_name*:parsed_value,.........}**. The keys of *new_feature_name* will be used used to create new parsed feature column. **Note** that the **new_feature_name**(s) in the above dictionary will replace the input feature in the **all_features_dict**.
4. Each parsing function that you want to use must be first defined and then passed as a list (or use the function object itself, if using only one) to **ParsingFunction.add_new_parsers()** as an argument.
