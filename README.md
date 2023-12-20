import pandas as pd
import requests

api_url="https://api.spacexdata.com/v4/launches"

def get_spacex_launches():
   response=requests.get(api_url)
   data=response.json()
   return data

spacex_launches=get_spacex_launches()
launch_data=[]
for launch in spacex_launches:
  rocket_name=launches['rocket'] if 'rocket' in launch and isinstance(launch['rocket'],dict)else{}
  launchpad_info=launch['launchpad']if 'launchpad' in launch and isinstance(launch['launchpad'],dict)else{}
  launch_info={
      'launch date': launch.get('date_utc'),
      'rocket name': rocket_name.get('name'),
      'launch site':launchpad_info.get('site_name_long'),
      'payload name':launch.get('name'),
      'success':launch.get('success'),
  }
  launch_data.append(launch_info)

df=pd.DataFrame(launch_data)
df.to_csv('spacex_launches_dataset.csv',index=False)
print(df)
