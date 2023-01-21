# Mongodb
## Load data into mongodb using python
## Using vega_datasets.data in python

## To load a sigle dataset

import vega_datasets
import pymongo
import time

connString='mongodb://sathesht11:sathesh123@ac-pdkcaeq-shard-00-00.'\
            'p3oqjnt.mongodb.net:27017,ac-pdkcaeq-shard-00-01.p3oqjnt.'\
            'mongodb.net:27017,ac-pdkcaeq-shard-00-02.p3oqjnt.'\
            'mongodb.net:27017/?ssl=true&replicaSet=atlas-jw3sl9-shard-'\
            '0&authSource=admin&retryWrites=true&w=majority'

def change_df_to_dict(df):
  data = df.to_dict('records')
  return data

def load_data_to_mongodb(df,connString,col_name):
  data = change_df_to_dict(df)
  conn = pymongo.MongoClient(connString)
  db = conn['vega_datasets']
  col = db[col_name]
  for i in data:
    col.insert_one(i)

start_time=time.time()
load_data_to_mongodb(vega_datasets.data.windvectors(),connString,'windvectors')
end_time=time.time()
print('windvectors_data completed')
print('Time taken:',(end_time-start_time)/60,'mins')
