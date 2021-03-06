=====
Data Queues
=====

dataqs (Data Queues) is a simple Django app to download, process,
and import spatial data into GeoServer/GeoNode.


Quick start
-----------

1. Add "dataqs" to your INSTALLED_APPS setting like this::

    INSTALLED_APPS = (
        ...
        'dataqs',
        'dataqs.forecastio',
        'dataqs.gfms',
    )
    
2. In your settings.py or local_settings.py file, add a CELERYBEAT_SCHEDULE
   setting to specify when Celery should run data_queues tasks::
   
   	from celery.schedules import crontab
	CELERYBEAT_SCHEDULE = {
	    'gfms': {
	        'task': 'dataqs.gfms.tasks.gfms_task',
	        'schedule': crontab(minute='3'),
	        'args': ()
	    },
	    'forecast_io': {
	        'task': 'dataqs.forecastio.tasks.forecast_io_task',
	        'schedule': crontab(minute='1'),
	        'args': ()
	    },
	}

  Also add the following settings::
  
	#Location of GeoServer data directory
	GS_DATA_DIR = '/usr/share/geoserver/data'
	#Directory where temporary dataqs geoprocessing files should be downloaded
	GS_TMP_DIR = GS_DATA_DIR + '/tmp'
	#Time to wait before updating Geoserver mosaic (keep at 0 unless Geoserver is on a different server.
	#In that case, there will need to be an automated rsync between GS_TMP_DIR where celery is running and
	#GS_DATA_DIR where GeoServer is running.
	RSYNC_WAIT_TIME = 0
