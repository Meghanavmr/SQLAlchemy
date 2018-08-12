

```python
%matplotlib notebook
from matplotlib import style
style.use('fivethirtyeight')
import matplotlib.pyplot as plt
```


```python
import numpy as np
import pandas as pd
```


```python
import datetime as dt
```

# Reflect Tables into SQLAlchemy ORM


```python
# Python SQL toolkit and Object Relational Mapper
import sqlalchemy
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session
from sqlalchemy import create_engine, func
```


```python
#engine = create_engine("sqlite:///../Resources/hawaii.sqlite")
engine = create_engine("sqlite:///Resources/hawaii.sqlite")
```


```python
# reflect an existing database into a new model
Base = automap_base()
# reflect the tables
Base.prepare(engine, reflect=True)
```


```python
# We can view all of the classes that automap found
Base.classes.keys()
```




    ['measurement', 'station']




```python
# Save references to each table
Measurement = Base.classes.measurement
Station = Base.classes.station
```


```python
# Create our session (link) from Python to the DB
session = Session(engine)
```

# Exploratory Climate Analysis


```python
first_row = session.query(Measurement).first()
first_row.__dict__
```




    {'_sa_instance_state': <sqlalchemy.orm.state.InstanceState at 0x22f85c912e8>,
     'date': '2010-01-01',
     'id': 1,
     'prcp': 0.08,
     'station': 'USC00519397',
     'tobs': 65.0}




```python
first_row = session.query(Station).first()
first_row.__dict__
```




    {'_sa_instance_state': <sqlalchemy.orm.state.InstanceState at 0x22f85c91c88>,
     'elevation': 3.0,
     'id': 1,
     'latitude': 21.2716,
     'longitude': -157.8168,
     'name': 'WAIKIKI 717.2, HI US',
     'station': 'USC00519397'}




```python
session.query(func.count(Measurement.id)).all()
```




    [(19550)]




```python
session.query(Measurement.date).order_by(Measurement.date.desc()).first()
#most recent data Aug 23, 2017
```




    ('2017-08-23')




```python
# Design a query to retrieve the last 12 months of precipitation data and plot the results
# Calculate the date 1 year ago from today
# Perform a query to retrieve the data and precipitation scores
# Save the query results as a Pandas DataFrame and set the index to the date column
# Sort the dataframe by date
# Use Pandas Plotting with Matplotlib to plot the data
# Rotate the xticks for the dates
##========
# Design a query to retrieve the last 12 months of precipitation data.
# Select only the date and prcp values.
# Load the query results into a Pandas DataFrame and set the index to the date column.
# Sort the DataFrame values by date
# Plot the results using the DataFrame plot method.
```


```python
sel = [Measurement.date, Measurement.prcp]
climate_data = session.query(*sel).\
    filter(Measurement.date > '2016-08-23').\
    order_by(Measurement.date).all()
climate_data
```




    [('2016-08-24', 0.08),
     ('2016-08-24', 2.15),
     ('2016-08-24', 2.28),
     ('2016-08-24', None),
     ('2016-08-24', 1.22),
     ('2016-08-24', 2.15),
     ('2016-08-24', 1.45),
     ('2016-08-25', 0.08),
     ('2016-08-25', 0.08),
     ('2016-08-25', 0.0),
     ('2016-08-25', 0.0),
     ('2016-08-25', 0.21),
     ('2016-08-25', 0.06),
     ('2016-08-25', 0.11),
     ('2016-08-26', 0.0),
     ('2016-08-26', 0.03),
     ('2016-08-26', 0.02),
     ('2016-08-26', 0.04),
     ('2016-08-26', 0.0),
     ('2016-08-26', 0.01),
     ('2016-08-27', 0.0),
     ('2016-08-27', 0.18),
     ('2016-08-27', 0.02),
     ('2016-08-27', 0.0),
     ('2016-08-27', 0.12),
     ('2016-08-27', None),
     ('2016-08-28', 0.01),
     ('2016-08-28', 0.14),
     ('2016-08-28', 0.14),
     ('2016-08-28', 0.14),
     ('2016-08-28', 0.6),
     ('2016-08-28', 2.07),
     ('2016-08-29', 0.0),
     ('2016-08-29', 0.17),
     ('2016-08-29', 0.04),
     ('2016-08-29', None),
     ('2016-08-29', 0.0),
     ('2016-08-29', 0.35),
     ('2016-08-29', 0.9),
     ('2016-08-30', 0.0),
     ('2016-08-30', 0.0),
     ('2016-08-30', 0.02),
     ('2016-08-30', 0.0),
     ('2016-08-30', 0.0),
     ('2016-08-30', 0.05),
     ('2016-08-31', 0.13),
     ('2016-08-31', 0.1),
     ('2016-08-31', None),
     ('2016-08-31', None),
     ('2016-08-31', 0.25),
     ('2016-08-31', 0.24),
     ('2016-08-31', 2.46),
     ('2016-09-01', 0.0),
     ('2016-09-01', 0.0),
     ('2016-09-01', 0.0),
     ('2016-09-01', None),
     ('2016-09-01', 0.02),
     ('2016-09-01', 0.01),
     ('2016-09-02', 0.0),
     ('2016-09-02', 0.02),
     ('2016-09-02', 0.19),
     ('2016-09-02', None),
     ('2016-09-02', None),
     ('2016-09-02', 0.01),
     ('2016-09-02', 0.03),
     ('2016-09-03', 0.0),
     ('2016-09-03', 0.07),
     ('2016-09-03', 0.08),
     ('2016-09-03', 0.12),
     ('2016-09-03', 1.0),
     ('2016-09-04', 0.03),
     ('2016-09-04', 0.03),
     ('2016-09-04', 0.74),
     ('2016-09-04', 0.14),
     ('2016-09-04', 0.44),
     ('2016-09-05', None),
     ('2016-09-05', 0.11),
     ('2016-09-05', None),
     ('2016-09-05', 0.02),
     ('2016-09-05', 0.03),
     ('2016-09-05', 0.18),
     ('2016-09-06', None),
     ('2016-09-06', 0.05),
     ('2016-09-06', 0.04),
     ('2016-09-06', 0.03),
     ('2016-09-06', 0.11),
     ('2016-09-06', 1.0),
     ('2016-09-07', 0.05),
     ('2016-09-07', 0.1),
     ('2016-09-07', 0.23),
     ('2016-09-07', 0.11),
     ('2016-09-07', 0.16),
     ('2016-09-07', 1.35),
     ('2016-09-08', 0.0),
     ('2016-09-08', 0.22),
     ('2016-09-08', 0.01),
     ('2016-09-08', None),
     ('2016-09-08', 0.01),
     ('2016-09-08', 0.07),
     ('2016-09-08', 0.15),
     ('2016-09-09', 0.03),
     ('2016-09-09', 0.01),
     ('2016-09-09', 0.29),
     ('2016-09-09', None),
     ('2016-09-09', 0.23),
     ('2016-09-09', 0.16),
     ('2016-09-09', 0.35),
     ('2016-09-10', 0.0),
     ('2016-09-10', 0.01),
     ('2016-09-10', 0.14),
     ('2016-09-10', 0.09),
     ('2016-09-10', 1.16),
     ('2016-09-11', 0.05),
     ('2016-09-11', 0.18),
     ('2016-09-11', 0.12),
     ('2016-09-11', 0.3),
     ('2016-09-11', 0.6),
     ('2016-09-12', 0.0),
     ('2016-09-12', 0.04),
     ('2016-09-12', None),
     ('2016-09-12', None),
     ('2016-09-12', 0.15),
     ('2016-09-12', 0.31),
     ('2016-09-12', 1.04),
     ('2016-09-13', 0.02),
     ('2016-09-13', 0.37),
     ('2016-09-13', 0.32),
     ('2016-09-13', None),
     ('2016-09-13', 0.46),
     ('2016-09-13', 0.34),
     ('2016-09-13', 1.2),
     ('2016-09-14', 1.32),
     ('2016-09-14', 0.9),
     ('2016-09-14', 1.84),
     ('2016-09-14', None),
     ('2016-09-14', 1.19),
     ('2016-09-14', 2.33),
     ('2016-09-14', 6.7),
     ('2016-09-15', 0.42),
     ('2016-09-15', 0.12),
     ('2016-09-15', 0.07),
     ('2016-09-15', None),
     ('2016-09-15', 0.17),
     ('2016-09-15', 0.83),
     ('2016-09-15', 3.35),
     ('2016-09-16', 0.06),
     ('2016-09-16', 0.01),
     ('2016-09-16', 0.07),
     ('2016-09-16', 0.0),
     ('2016-09-16', 0.01),
     ('2016-09-16', 0.06),
     ('2016-09-16', 0.61),
     ('2016-09-17', 0.05),
     ('2016-09-17', 0.04),
     ('2016-09-17', 0.0),
     ('2016-09-17', 0.36),
     ('2016-09-17', 0.23),
     ('2016-09-18', 0.0),
     ('2016-09-18', 0.0),
     ('2016-09-18', 0.04),
     ('2016-09-18', 0.07),
     ('2016-09-18', 0.42),
     ('2016-09-19', 0.0),
     ('2016-09-19', 0.01),
     ('2016-09-19', None),
     ('2016-09-19', None),
     ('2016-09-19', 0.05),
     ('2016-09-19', 0.01),
     ('2016-09-19', 0.25),
     ('2016-09-20', 0.0),
     ('2016-09-20', 0.09),
     ('2016-09-20', 0.25),
     ('2016-09-20', 0.0),
     ('2016-09-20', 0.04),
     ('2016-09-20', 0.22),
     ('2016-09-20', 0.43),
     ('2016-09-21', 0.0),
     ('2016-09-21', 0.06),
     ('2016-09-21', 0.02),
     ('2016-09-21', 0.0),
     ('2016-09-21', 0.07),
     ('2016-09-21', 1.02),
     ('2016-09-22', 0.02),
     ('2016-09-22', 0.09),
     ('2016-09-22', 0.17),
     ('2016-09-22', 0.06),
     ('2016-09-22', 0.01),
     ('2016-09-22', 0.34),
     ('2016-09-22', 0.75),
     ('2016-09-23', 0.0),
     ('2016-09-23', 0.15),
     ('2016-09-23', 0.15),
     ('2016-09-23', 0.0),
     ('2016-09-23', 0.0),
     ('2016-09-23', 0.94),
     ('2016-09-23', 0.33),
     ('2016-09-24', 0.0),
     ('2016-09-24', 0.0),
     ('2016-09-24', 0.0),
     ('2016-09-24', 0.0),
     ('2016-09-24', 0.01),
     ('2016-09-24', 0.27),
     ('2016-09-25', 0.0),
     ('2016-09-25', 0.02),
     ('2016-09-25', 0.0),
     ('2016-09-25', 0.0),
     ('2016-09-25', 0.03),
     ('2016-09-25', 0.04),
     ('2016-09-26', 0.06),
     ('2016-09-26', 0.06),
     ('2016-09-26', 0.02),
     ('2016-09-26', None),
     ('2016-09-26', 0.34),
     ('2016-09-26', 0.17),
     ('2016-09-26', 1.02),
     ('2016-09-27', 0.02),
     ('2016-09-27', 0.12),
     ('2016-09-27', 0.0),
     ('2016-09-27', 0.05),
     ('2016-09-27', 0.17),
     ('2016-09-27', 1.0),
     ('2016-09-28', 0.0),
     ('2016-09-28', 0.08),
     ('2016-09-28', 0.0),
     ('2016-09-28', 0.0),
     ('2016-09-28', 0.0),
     ('2016-09-28', 0.0),
     ('2016-09-28', 0.05),
     ('2016-09-29', 0.0),
     ('2016-09-29', 0.49),
     ('2016-09-29', 0.2),
     ('2016-09-29', 0.04),
     ('2016-09-29', 0.18),
     ('2016-09-29', 0.59),
     ('2016-09-29', 1.49),
     ('2016-09-30', 0.0),
     ('2016-09-30', 0.31),
     ('2016-09-30', 0.06),
     ('2016-09-30', None),
     ('2016-09-30', 0.15),
     ('2016-09-30', 0.25),
     ('2016-09-30', 0.38),
     ('2016-10-01', 0.0),
     ('2016-10-01', 0.14),
     ('2016-10-01', 0.08),
     ('2016-10-01', 0.07),
     ('2016-10-01', 0.14),
     ('2016-10-01', 1.02),
     ('2016-10-02', 0.0),
     ('2016-10-02', 0.02),
     ('2016-10-02', 0.03),
     ('2016-10-02', 0.0),
     ('2016-10-02', 0.06),
     ('2016-10-02', 0.61),
     ('2016-10-03', 0.0),
     ('2016-10-03', 0.04),
     ('2016-10-03', 0.03),
     ('2016-10-03', None),
     ('2016-10-03', 0.0),
     ('2016-10-03', 0.16),
     ('2016-10-03', 0.46),
     ('2016-10-04', 0.0),
     ('2016-10-04', 0.0),
     ('2016-10-04', 0.0),
     ('2016-10-04', None),
     ('2016-10-04', 0.0),
     ('2016-10-04', 0.03),
     ('2016-10-04', 3.46),
     ('2016-10-05', 0.0),
     ('2016-10-05', 0.0),
     ('2016-10-05', 0.0),
     ('2016-10-05', None),
     ('2016-10-05', 0.0),
     ('2016-10-05', 0.01),
     ('2016-10-05', 0.81),
     ('2016-10-06', 0.0),
     ('2016-10-06', 0.05),
     ('2016-10-06', 0.0),
     ('2016-10-06', 0.07),
     ('2016-10-06', 0.0),
     ('2016-10-06', 0.0),
     ('2016-10-06', 0.04),
     ('2016-10-07', 0.0),
     ('2016-10-07', 0.0),
     ('2016-10-07', 0.0),
     ('2016-10-07', None),
     ('2016-10-07', 0.0),
     ('2016-10-07', 0.0),
     ('2016-10-07', 0.01),
     ('2016-10-08', 0.0),
     ('2016-10-08', 0.0),
     ('2016-10-08', 0.0),
     ('2016-10-08', 0.0),
     ('2016-10-08', 0.04),
     ('2016-10-09', 0.0),
     ('2016-10-09', 0.0),
     ('2016-10-09', 0.0),
     ('2016-10-09', 0.0),
     ('2016-10-09', 0.0),
     ('2016-10-10', 0.0),
     ('2016-10-10', 0.0),
     ('2016-10-10', None),
     ('2016-10-10', 0.0),
     ('2016-10-10', 0.0),
     ('2016-10-10', 0.0),
     ('2016-10-11', 0.0),
     ('2016-10-11', 0.02),
     ('2016-10-11', 0.04),
     ('2016-10-11', None),
     ('2016-10-11', 0.0),
     ('2016-10-11', 0.28),
     ('2016-10-11', 0.35),
     ('2016-10-12', 0.0),
     ('2016-10-12', 0.03),
     ('2016-10-12', 0.0),
     ('2016-10-12', 0.0),
     ('2016-10-12', 0.03),
     ('2016-10-12', 0.02),
     ('2016-10-13', 0.0),
     ('2016-10-13', 0.0),
     ('2016-10-13', 0.02),
     ('2016-10-13', None),
     ('2016-10-13', 0.0),
     ('2016-10-13', 0.0),
     ('2016-10-13', 0.06),
     ('2016-10-14', 0.0),
     ('2016-10-14', 0.0),
     ('2016-10-14', 0.0),
     ('2016-10-14', 0.0),
     ('2016-10-14', 0.0),
     ('2016-10-14', 0.0),
     ('2016-10-15', 0.0),
     ('2016-10-15', 0.0),
     ('2016-10-15', 0.02),
     ('2016-10-15', 0.0),
     ('2016-10-15', 0.04),
     ('2016-10-15', 0.33),
     ('2016-10-16', 0.0),
     ('2016-10-16', 0.0),
     ('2016-10-16', 0.0),
     ('2016-10-16', 0.0),
     ('2016-10-16', 0.0),
     ('2016-10-17', 0.01),
     ('2016-10-17', 0.03),
     ('2016-10-17', None),
     ('2016-10-17', None),
     ('2016-10-17', 0.12),
     ('2016-10-17', 0.01),
     ('2016-10-17', 0.38),
     ('2016-10-18', 0.0),
     ('2016-10-18', 0.05),
     ('2016-10-18', 0.03),
     ('2016-10-18', None),
     ('2016-10-18', 0.02),
     ('2016-10-18', 0.02),
     ('2016-10-18', 0.48),
     ('2016-10-19', 0.0),
     ('2016-10-19', 0.06),
     ('2016-10-19', 0.0),
     ('2016-10-19', None),
     ('2016-10-19', 0.0),
     ('2016-10-19', 0.11),
     ('2016-10-19', 0.0),
     ('2016-10-20', 0.0),
     ('2016-10-20', 0.0),
     ('2016-10-20', 0.01),
     ('2016-10-20', None),
     ('2016-10-20', 0.0),
     ('2016-10-20', 1.0),
     ('2016-10-21', 0.05),
     ('2016-10-21', 0.15),
     ('2016-10-21', 0.03),
     ('2016-10-21', None),
     ('2016-10-21', None),
     ('2016-10-21', 0.0),
     ('2016-10-21', 0.09),
     ('2016-10-22', 0.15),
     ('2016-10-22', 0.1),
     ('2016-10-22', 0.0),
     ('2016-10-22', 0.15),
     ('2016-10-22', 1.37),
     ('2016-10-23', 0.01),
     ('2016-10-23', 0.01),
     ('2016-10-23', None),
     ('2016-10-23', 0.0),
     ('2016-10-23', 0.02),
     ('2016-10-23', 0.24),
     ('2016-10-24', 0.0),
     ('2016-10-24', 0.0),
     ('2016-10-24', 0.01),
     ('2016-10-24', None),
     ('2016-10-24', 0.0),
     ('2016-10-24', 0.08),
     ('2016-10-24', 0.7),
     ('2016-10-25', 0.03),
     ('2016-10-25', 0.04),
     ('2016-10-25', 0.0),
     ('2016-10-25', 0.4),
     ('2016-10-25', 0.12),
     ('2016-10-25', 0.11),
     ('2016-10-25', 0.4),
     ('2016-10-26', 0.0),
     ('2016-10-26', 0.06),
     ('2016-10-26', 0.2),
     ('2016-10-26', 0.02),
     ('2016-10-26', 0.01),
     ('2016-10-26', 0.0),
     ('2016-10-27', 0.0),
     ('2016-10-27', 0.11),
     ('2016-10-27', 0.2),
     ('2016-10-27', None),
     ('2016-10-27', 0.08),
     ('2016-10-27', 0.22),
     ('2016-10-27', 1.25),
     ('2016-10-28', 0.0),
     ('2016-10-28', 0.02),
     ('2016-10-28', 0.07),
     ('2016-10-28', None),
     ('2016-10-28', 0.06),
     ('2016-10-28', 0.05),
     ('2016-10-28', 0.37),
     ('2016-10-29', 0.0),
     ('2016-10-29', 0.02),
     ('2016-10-29', 0.26),
     ('2016-10-29', 0.01),
     ('2016-10-29', 0.1),
     ('2016-10-29', 0.25),
     ('2016-10-30', 0.24),
     ('2016-10-30', 0.1),
     ('2016-10-30', 0.14),
     ('2016-10-30', 0.0),
     ('2016-10-30', 0.16),
     ('2016-10-30', 0.95),
     ('2016-10-31', 0.03),
     ('2016-10-31', 0.03),
     ('2016-10-31', 0.0),
     ('2016-10-31', None),
     ('2016-10-31', 0.13),
     ('2016-10-31', 0.07),
     ('2016-10-31', 1.35),
     ('2016-11-01', 0.0),
     ('2016-11-01', 0.01),
     ('2016-11-01', 0.0),
     ('2016-11-01', 0.01),
     ('2016-11-01', 0.1),
     ('2016-11-01', 0.09),
     ('2016-11-02', 0.0),
     ('2016-11-02', 0.0),
     ('2016-11-02', 0.0),
     ('2016-11-02', 0.0),
     ('2016-11-02', 0.0),
     ('2016-11-02', 0.04),
     ('2016-11-03', 0.0),
     ('2016-11-03', 0.0),
     ('2016-11-03', 0.0),
     ('2016-11-03', 0.0),
     ('2016-11-03', 0.0),
     ('2016-11-03', 0.02),
     ('2016-11-04', 0.0),
     ('2016-11-04', 0.0),
     ('2016-11-04', 0.0),
     ('2016-11-04', None),
     ('2016-11-04', 0.0),
     ('2016-11-04', 0.0),
     ('2016-11-04', 0.06),
     ('2016-11-05', 0.0),
     ('2016-11-05', 0.02),
     ('2016-11-05', 0.0),
     ('2016-11-05', 0.02),
     ('2016-11-05', 0.03),
     ('2016-11-05', 0.38),
     ('2016-11-06', 0.0),
     ('2016-11-06', 0.02),
     ('2016-11-06', 0.0),
     ('2016-11-06', 0.0),
     ('2016-11-06', 0.01),
     ('2016-11-06', 0.05),
     ('2016-11-07', 0.0),
     ('2016-11-07', 0.0),
     ('2016-11-07', 0.13),
     ('2016-11-07', None),
     ('2016-11-07', 0.0),
     ('2016-11-07', 0.0),
     ('2016-11-07', 0.05),
     ('2016-11-08', 0.07),
     ('2016-11-08', 0.14),
     ('2016-11-08', 0.02),
     ('2016-11-08', 0.15),
     ('2016-11-08', 0.21),
     ('2016-11-08', 0.53),
     ('2016-11-09', 0.0),
     ('2016-11-09', 0.08),
     ('2016-11-09', 0.17),
     ('2016-11-09', 0.0),
     ('2016-11-09', 0.0),
     ('2016-11-09', 0.11),
     ('2016-11-09', 0.04),
     ('2016-11-10', 0.0),
     ('2016-11-10', 0.0),
     ('2016-11-10', 0.0),
     ('2016-11-10', 0.0),
     ('2016-11-10', 0.0),
     ('2016-11-10', 0.01),
     ('2016-11-11', 0.0),
     ('2016-11-11', 0.0),
     ('2016-11-11', 0.0),
     ('2016-11-11', 0.0),
     ('2016-11-11', 0.0),
     ('2016-11-11', 0.0),
     ('2016-11-12', 0.0),
     ('2016-11-12', 0.0),
     ('2016-11-12', 0.0),
     ('2016-11-12', 0.0),
     ('2016-11-12', 0.0),
     ('2016-11-12', 0.0),
     ('2016-11-13', 0.0),
     ('2016-11-13', 0.0),
     ('2016-11-13', 0.0),
     ('2016-11-13', 0.0),
     ('2016-11-13', 0.0),
     ('2016-11-13', 0.0),
     ('2016-11-14', 0.0),
     ('2016-11-14', 0.06),
     ('2016-11-14', 0.05),
     ('2016-11-14', 0.02),
     ('2016-11-14', 0.0),
     ('2016-11-14', 0.0),
     ('2016-11-14', 0.02),
     ('2016-11-15', 0.0),
     ('2016-11-15', 0.0),
     ('2016-11-15', 0.0),
     ('2016-11-15', None),
     ('2016-11-15', 0.0),
     ('2016-11-15', 0.0),
     ('2016-11-15', 0.05),
     ('2016-11-16', 0.0),
     ('2016-11-16', 0.14),
     ('2016-11-16', 0.18),
     ('2016-11-16', None),
     ('2016-11-16', 0.07),
     ('2016-11-16', 0.24),
     ('2016-11-16', 0.91),
     ('2016-11-17', 0.0),
     ('2016-11-17', 0.03),
     ('2016-11-17', 0.0),
     ('2016-11-17', None),
     ('2016-11-17', 0.0),
     ('2016-11-17', 0.01),
     ('2016-11-17', 0.02),
     ('2016-11-18', 0.0),
     ('2016-11-18', 0.01),
     ('2016-11-18', None),
     ('2016-11-18', 0.02),
     ('2016-11-18', 0.0),
     ('2016-11-19', 0.03),
     ('2016-11-19', 0.11),
     ('2016-11-19', 0.13),
     ('2016-11-19', 0.11),
     ('2016-11-20', 0.05),
     ('2016-11-20', 0.11),
     ('2016-11-20', 0.4),
     ('2016-11-20', 0.39),
     ('2016-11-20', None),
     ('2016-11-21', 0.01),
     ('2016-11-21', 0.02),
     ('2016-11-21', None),
     ('2016-11-21', 0.07),
     ('2016-11-21', 0.11),
     ('2016-11-21', 2.87),
     ('2016-11-22', 0.13),
     ('2016-11-22', 0.41),
     ('2016-11-22', None),
     ('2016-11-22', None),
     ('2016-11-22', 0.31),
     ('2016-11-22', 2.05),
     ('2016-11-22', 2.11),
     ('2016-11-23', 0.14),
     ('2016-11-23', 0.03),
     ('2016-11-23', None),
     ('2016-11-23', 0.03),
     ('2016-11-23', 0.25),
     ('2016-11-23', 0.22),
     ('2016-11-24', 0.05),
     ('2016-11-24', 0.2),
     ('2016-11-24', 0.21),
     ('2016-11-24', 0.3),
     ('2016-11-24', 0.72),
     ('2016-11-25', 0.05),
     ('2016-11-25', 0.05),
     ('2016-11-25', None),
     ('2016-11-25', 0.11),
     ('2016-11-25', 0.08),
     ('2016-11-25', 1.03),
     ('2016-11-26', 0.05),
     ('2016-11-26', 0.05),
     ('2016-11-26', 0.02),
     ('2016-11-26', 0.03),
     ('2016-11-26', 0.06),
     ('2016-11-26', 0.3),
     ('2016-11-27', 0.0),
     ('2016-11-27', 0.06),
     ('2016-11-27', 0.03),
     ('2016-11-27', 0.0),
     ('2016-11-27', 0.17),
     ('2016-11-27', 0.29),
     ('2016-11-28', 0.01),
     ('2016-11-28', 0.02),
     ('2016-11-28', 0.0),
     ('2016-11-28', None),
     ('2016-11-28', 0.0),
     ('2016-11-28', 0.0),
     ('2016-11-28', 0.69),
     ('2016-11-29', 0.0),
     ('2016-11-29', 0.04),
     ('2016-11-29', 0.04),
     ('2016-11-29', None),
     ('2016-11-29', 0.06),
     ('2016-11-29', 0.09),
     ('2016-11-29', 0.2),
     ('2016-11-30', 0.14),
     ('2016-11-30', 0.05),
     ('2016-11-30', 0.03),
     ('2016-11-30', None),
     ('2016-11-30', 0.0),
     ('2016-11-30', 0.05),
     ('2016-11-30', 0.79),
     ('2016-12-01', 0.12),
     ('2016-12-01', 0.33),
     ('2016-12-01', 0.07),
     ('2016-12-01', None),
     ('2016-12-01', 0.16),
     ('2016-12-01', 0.37),
     ('2016-12-01', 0.72),
     ('2016-12-02', 0.03),
     ('2016-12-02', 0.3),
     ('2016-12-02', 0.4),
     ('2016-12-02', None),
     ('2016-12-02', 0.01),
     ('2016-12-02', 0.35),
     ('2016-12-02', 1.27),
     ('2016-12-03', 0.0),
     ('2016-12-03', 0.04),
     ('2016-12-03', 0.26),
     ('2016-12-03', 0.02),
     ('2016-12-03', 0.77),
     ('2016-12-03', 1.62),
     ('2016-12-04', 0.03),
     ('2016-12-04', 0.1),
     ('2016-12-04', 0.0),
     ('2016-12-04', 0.32),
     ('2016-12-04', 0.04),
     ('2016-12-04', 0.31),
     ('2016-12-05', 0.43),
     ('2016-12-05', 0.34),
     ('2016-12-05', 0.2),
     ('2016-12-05', None),
     ('2016-12-05', 0.45),
     ('2016-12-05', 0.22),
     ('2016-12-05', 1.6),
     ('2016-12-06', 0.02),
     ('2016-12-06', 0.02),
     ('2016-12-06', None),
     ('2016-12-06', 0.0),
     ('2016-12-06', 0.0),
     ('2016-12-06', 0.0),
     ('2016-12-07', 0.0),
     ('2016-12-07', 0.17),
     ('2016-12-07', None),
     ('2016-12-07', None),
     ('2016-12-07', 0.07),
     ('2016-12-07', 0.12),
     ('2016-12-07', 0.02),
     ('2016-12-08', 0.03),
     ('2016-12-08', 0.03),
     ('2016-12-08', 0.02),
     ('2016-12-08', 0.27),
     ('2016-12-08', 0.01),
     ('2016-12-08', 0.07),
     ('2016-12-08', 0.03),
     ('2016-12-09', 0.52),
     ('2016-12-09', 0.34),
     ('2016-12-09', 0.26),
     ('2016-12-09', None),
     ('2016-12-09', 0.31),
     ('2016-12-09', 0.42),
     ('2016-12-10', 0.05),
     ('2016-12-10', 0.02),
     ('2016-12-10', 0.0),
     ('2016-12-10', None),
     ('2016-12-10', 0.02),
     ('2016-12-10', 0.04),
     ('2016-12-11', 0.04),
     ('2016-12-11', 0.02),
     ('2016-12-11', 0.06),
     ('2016-12-11', 0.0),
     ('2016-12-11', 0.13),
     ('2016-12-12', 0.01),
     ('2016-12-12', 0.01),
     ('2016-12-12', None),
     ('2016-12-12', 0.02),
     ('2016-12-12', 0.0),
     ('2016-12-12', 0.0),
     ('2016-12-12', 0.01),
     ('2016-12-13', 0.05),
     ('2016-12-13', 0.1),
     ('2016-12-13', 0.34),
     ('2016-12-13', None),
     ('2016-12-13', 0.15),
     ('2016-12-13', 0.04),
     ('2016-12-13', 0.09),
     ('2016-12-14', 0.03),
     ('2016-12-14', 0.05),
     ('2016-12-14', 0.12),
     ('2016-12-14', None),
     ('2016-12-14', 0.05),
     ('2016-12-14', 0.92),
     ('2016-12-14', 0.33),
     ('2016-12-15', 0.0),
     ('2016-12-15', 0.02),
     ('2016-12-15', 0.07),
     ('2016-12-15', None),
     ('2016-12-15', 0.0),
     ('2016-12-15', 0.14),
     ('2016-12-15', 0.03),
     ('2016-12-16', 0.0),
     ('2016-12-16', 0.01),
     ('2016-12-16', 0.0),
     ('2016-12-16', None),
     ('2016-12-16', 0.0),
     ('2016-12-16', 0.03),
     ('2016-12-16', 0.0),
     ('2016-12-17', 0.01),
     ('2016-12-17', 0.11),
     ('2016-12-17', 0.0),
     ('2016-12-17', 0.16),
     ('2016-12-17', 0.07),
     ('2016-12-18', 0.13),
     ('2016-12-18', 0.29),
     ('2016-12-18', 0.04),
     ('2016-12-18', 0.27),
     ('2016-12-18', 0.16),
     ('2016-12-18', None),
     ('2016-12-19', 0.01),
     ('2016-12-19', 0.21),
     ('2016-12-19', 0.0),
     ('2016-12-19', None),
     ('2016-12-19', 0.02),
     ('2016-12-19', 0.03),
     ('2016-12-19', 0.15),
     ('2016-12-20', 0.0),
     ('2016-12-20', 0.02),
     ('2016-12-20', 0.0),
     ('2016-12-20', None),
     ('2016-12-20', 0.01),
     ('2016-12-20', 0.0),
     ('2016-12-20', 0.0),
     ('2016-12-21', 0.0),
     ('2016-12-21', 0.03),
     ('2016-12-21', 0.09),
     ('2016-12-21', 0.06),
     ('2016-12-21', 0.06),
     ('2016-12-21', 0.11),
     ('2016-12-21', 0.55),
     ('2016-12-22', 0.01),
     ('2016-12-22', 0.17),
     ('2016-12-22', 0.05),
     ('2016-12-22', None),
     ('2016-12-22', 0.14),
     ('2016-12-22', 0.86),
     ('2016-12-22', 1.24),
     ('2016-12-23', 0.01),
     ('2016-12-23', 0.1),
     ('2016-12-23', 0.03),
     ('2016-12-23', None),
     ('2016-12-23', 0.02),
     ('2016-12-23', 0.24),
     ('2016-12-23', 0.83),
     ('2016-12-24', 0.01),
     ('2016-12-24', 0.14),
     ('2016-12-24', 0.13),
     ('2016-12-24', 0.06),
     ('2016-12-24', 0.2),
     ('2016-12-24', 1.08),
     ('2016-12-25', 0.0),
     ('2016-12-25', 0.03),
     ('2016-12-25', 0.0),
     ('2016-12-25', 0.02),
     ('2016-12-25', 0.38),
     ('2016-12-26', 0.02),
     ('2016-12-26', 0.26),
     ('2016-12-26', None),
     ('2016-12-26', 0.06),
     ('2016-12-26', 0.22),
     ('2016-12-26', 1.48),
     ('2016-12-27', 0.0),
     ('2016-12-27', 0.03),
     ('2016-12-27', 0.02),
     ('2016-12-27', 0.0),
     ('2016-12-27', 0.05),
     ('2016-12-27', 0.14),
     ('2016-12-28', 0.02),
     ('2016-12-28', 0.09),
     ('2016-12-28', 0.01),
     ('2016-12-28', None),
     ('2016-12-28', 0.06),
     ('2016-12-28', 0.09),
     ('2016-12-28', 0.14),
     ('2016-12-29', 0.04),
     ('2016-12-29', 0.18),
     ('2016-12-29', 0.56),
     ('2016-12-29', None),
     ('2016-12-29', 0.05),
     ('2016-12-29', 0.52),
     ('2016-12-29', 1.03),
     ('2016-12-30', 0.12),
     ('2016-12-30', 0.21),
     ('2016-12-30', 0.29),
     ('2016-12-30', None),
     ('2016-12-30', 0.07),
     ('2016-12-30', 0.29),
     ('2016-12-30', 2.37),
     ('2016-12-31', 0.01),
     ('2016-12-31', 0.62),
     ('2016-12-31', 0.36),
     ('2016-12-31', 0.25),
     ('2016-12-31', 0.9),
     ('2017-01-01', 0.0),
     ('2017-01-01', 0.29),
     ('2017-01-01', 0.0),
     ('2017-01-01', None),
     ('2017-01-01', 0.03),
     ('2017-01-01', 0.03),
     ('2017-01-02', 0.0),
     ('2017-01-02', 0.0),
     ('2017-01-02', 0.01),
     ('2017-01-02', 0.01),
     ('2017-01-02', 0.0),
     ('2017-01-03', 0.0),
     ('2017-01-03', 0.0),
     ('2017-01-03', 0.0),
     ('2017-01-03', None),
     ('2017-01-03', 0.0),
     ('2017-01-03', 0.0),
     ('2017-01-04', 0.0),
     ('2017-01-04', 0.0),
     ('2017-01-04', 0.0),
     ('2017-01-04', 0.18),
     ('2017-01-04', 0.0),
     ('2017-01-04', 0.0),
     ('2017-01-05', 0.0),
     ('2017-01-05', 0.0),
     ('2017-01-05', 0.0),
     ('2017-01-05', 0.42),
     ('2017-01-05', 0.06),
     ('2017-01-05', 0.47),
     ('2017-01-06', 0.0),
     ('2017-01-06', 0.0),
     ('2017-01-06', 0.59),
     ('2017-01-06', 0.01),
     ('2017-01-06', 0.1),
     ('2017-01-06', 0.1),
     ('2017-01-07', 0.0),
     ('2017-01-07', 0.06),
     ('2017-01-07', 0.0),
     ('2017-01-07', 0.0),
     ('2017-01-07', 0.0),
     ('2017-01-07', 0.0),
     ('2017-01-08', 0.0),
     ('2017-01-08', 0.0),
     ('2017-01-08', 0.03),
     ('2017-01-08', 0.0),
     ('2017-01-08', 0.0),
     ('2017-01-08', 0.03),
     ('2017-01-09', 0.0),
     ('2017-01-09', 0.0),
     ('2017-01-09', 0.0),
     ('2017-01-09', None),
     ('2017-01-09', 0.0),
     ('2017-01-09', 0.0),
     ('2017-01-09', 0.0),
     ('2017-01-10', 0.0),
     ('2017-01-10', 0.0),
     ('2017-01-10', 0.0),
     ('2017-01-10', None),
     ('2017-01-10', 0.0),
     ('2017-01-10', 0.0),
     ('2017-01-10', 0.0),
     ('2017-01-11', 0.0),
     ('2017-01-11', 0.0),
     ('2017-01-11', 0.0),
     ('2017-01-11', None),
     ('2017-01-11', 0.0),
     ('2017-01-11', 0.0),
     ('2017-01-12', 0.0),
     ('2017-01-12', 0.0),
     ('2017-01-12', None),
     ('2017-01-12', None),
     ('2017-01-12', 0.0),
     ('2017-01-12', 0.0),
     ('2017-01-13', 0.0),
     ('2017-01-13', 0.0),
     ('2017-01-13', None),
     ('2017-01-13', None),
     ('2017-01-13', 0.0),
     ('2017-01-13', 0.0),
     ('2017-01-14', 0.0),
     ('2017-01-14', 0.0),
     ('2017-01-14', 0.0),
     ('2017-01-14', 0.01),
     ('2017-01-14', 0.0),
     ('2017-01-15', 0.0),
     ('2017-01-15', 0.0),
     ('2017-01-15', None),
     ('2017-01-15', 0.0),
     ('2017-01-15', 0.01),
     ('2017-01-16', 0.0),
     ('2017-01-16', 0.0),
     ('2017-01-16', None),
     ('2017-01-16', 0.0),
     ('2017-01-16', 0.0),
     ('2017-01-16', 0.0),
     ('2017-01-17', 0.0),
     ('2017-01-17', 0.0),
     ('2017-01-17', 0.0),
     ('2017-01-17', 0.0),
     ('2017-01-17', 0.0),
     ('2017-01-17', 0.0),
     ('2017-01-18', 0.0),
     ('2017-01-18', 0.0),
     ('2017-01-18', 0.0),
     ('2017-01-18', 0.0),
     ('2017-01-18', None),
     ('2017-01-18', 0.0),
     ('2017-01-18', 0.07),
     ('2017-01-19', 0.0),
     ('2017-01-19', 0.0),
     ('2017-01-19', 0.0),
     ('2017-01-19', None),
     ('2017-01-19', 0.0),
     ('2017-01-19', 0.02),
     ('2017-01-19', 0.0),
     ('2017-01-20', 0.0),
     ('2017-01-20', 0.0),
     ('2017-01-20', 0.0),
     ('2017-01-20', None),
     ('2017-01-20', 0.0),
     ('2017-01-20', 0.0),
     ('2017-01-20', 0.0),
     ('2017-01-21', 0.0),
     ('2017-01-21', 0.04),
     ('2017-01-21', 0.02),
     ('2017-01-21', 0.11),
     ('2017-01-21', 0.03),
     ('2017-01-21', 0.08),
     ('2017-01-22', 0.16),
     ('2017-01-22', 0.01),
     ('2017-01-22', 0.04),
     ('2017-01-22', 0.09),
     ('2017-01-22', 0.72),
     ('2017-01-23', 0.0),
     ('2017-01-23', 0.08),
     ('2017-01-23', None),
     ('2017-01-23', None),
     ('2017-01-23', 0.0),
     ('2017-01-23', 0.01),
     ('2017-01-23', 0.85),
     ('2017-01-24', 0.04),
     ('2017-01-24', 0.15),
     ('2017-01-24', None),
     ('2017-01-24', 0.08),
     ('2017-01-24', 0.13),
     ('2017-01-24', 1.85),
     ('2017-01-25', 0.03),
     ('2017-01-25', 0.12),
     ('2017-01-25', None),
     ('2017-01-25', None),
     ('2017-01-25', 0.0),
     ('2017-01-25', 0.79),
     ('2017-01-25', 2.64),
     ('2017-01-26', 0.0),
     ('2017-01-26', 0.0),
     ('2017-01-26', 0.01),
     ('2017-01-26', 0.0),
     ('2017-01-26', 0.0),
     ('2017-01-26', 0.0),
     ('2017-01-26', 0.1),
     ('2017-01-27', 0.0),
     ('2017-01-27', 0.0),
     ('2017-01-27', 0.0),
     ('2017-01-27', 0.0),
     ('2017-01-27', 0.0),
     ('2017-01-27', 0.03),
     ('2017-01-27', 0.03),
     ('2017-01-28', 0.0),
     ('2017-01-28', 0.14),
     ('2017-01-28', 0.0),
     ('2017-01-28', 0.0),
     ('2017-01-28', 0.0),
     ('2017-01-29', 0.18),
     ('2017-01-29', 0.0),
     ...]




```python
len(climate_data)
# 2223
```




    2223




```python
climate_df = pd.DataFrame(climate_data, columns=['date', 'Precipitation'])
climate_df = climate_df.sort_values('date', ascending=True)
climate_df.set_index('date', inplace=True)
climate_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Precipitation</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-08-24</th>
      <td>0.08</td>
    </tr>
    <tr>
      <th>2016-08-24</th>
      <td>2.15</td>
    </tr>
    <tr>
      <th>2016-08-24</th>
      <td>2.28</td>
    </tr>
    <tr>
      <th>2016-08-24</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2016-08-24</th>
      <td>1.22</td>
    </tr>
  </tbody>
</table>
</div>




```python
# climate_df_srt_ind = pd.DataFrame(climate_df_srt, columns=['date', 'Precipitation'])
# climate_df_srt.set_index('date', inplace=True)
# climate_df_srt_ind.head()
```


```python
climate_df.plot(kind ="line", figsize=(8,6))
plt.yticks(np.arange(0, 4.5, step=0.5))
plt.legend(loc="upper left")
plt.tight_layout()
plt.show()

plt.savefig("hawaii_vacay.png",bbox_inches="tight")
```


    <IPython.core.display.Javascript object>



<img src="data:image/png;base64,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" width="800">



```python
# Use Pandas to calcualte the summary statistics for the precipitation data
climate_df.describe()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2015.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.176462</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.460288</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.020000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.130000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>6.700000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# How many stations are available in this dataset?



session.query(func.count(Station.station)).all()


```




    [(9)]




```python
session.query(Measurement.station, Station.station).limit(10).all()
```




    [('USC00519397', 'USC00519397'),
     ('USC00519397', 'USC00513117'),
     ('USC00519397', 'USC00514830'),
     ('USC00519397', 'USC00517948'),
     ('USC00519397', 'USC00518838'),
     ('USC00519397', 'USC00519523'),
     ('USC00519397', 'USC00519281'),
     ('USC00519397', 'USC00511918'),
     ('USC00519397', 'USC00516128'),
     ('USC00519397', 'USC00519397')]




```python
# What are the most active stations?
# List the stations and the counts in descending order.
#func.max, func.avg, and func.count 
from sqlalchemy import desc
station_count = [Measurement.station,
                func.count(Measurement.station).label("for_count")]

active_stations_desc = session.query(*station_count).\
                        group_by(Measurement.station).\
                        order_by(desc("for_count")).all()
        
active_stations_desc

```




    [('USC00519281', 2772),
     ('USC00519397', 2724),
     ('USC00513117', 2709),
     ('USC00519523', 2669),
     ('USC00516128', 2612),
     ('USC00514830', 2202),
     ('USC00511918', 1979),
     ('USC00517948', 1372),
     ('USC00518838', 511)]




```python
# Using the station id from the previous query, creating obejct
(station_max, count_max) = active_stations_desc[0]
print(station_max, count_max)
```

    USC00519281 2772
    


```python
# Using the station id from the previous query, calculate the lowest temperature recorded, 
# highest temperature recorded, and average temperature most active station?

temp_stats = session.query(func.min(Measurement.tobs), 
                           func.avg(Measurement.tobs),
                           func.max(Measurement.tobs)).\
                           filter(Measurement.station == station_max).\
                            all()
        
temp_stats
        

```




    [(54.0, 71.66378066378067, 85.0)]




```python
# Choose the station with the highest number of temperature observations.
# Query the last 12 months of temperature observation data for this station and plot the results as a histogram

```


```python
# Write a function called `calc_temps` that will accept start date and end date in the format '%Y-%m-%d' 
# and return the minimum, average, and maximum temperatures for that range of dates
def calc_temps(start_date, end_date):
    """TMIN, TAVG, and TMAX for a list of dates.
    
    Args:
        start_date (string): A date string in the format %Y-%m-%d
        end_date (string): A date string in the format %Y-%m-%d
        
    Returns:
        TMIN, TAVE, and TMAX
    """
    
    return session.query(func.min(Measurement.tobs), func.avg(Measurement.tobs), func.max(Measurement.tobs)).\
        filter(Measurement.date >= start_date).filter(Measurement.date <= end_date).all()
print(calc_temps('2012-02-28', '2012-03-05'))
```


```python
# Use your previous function `calc_temps` to calculate the tmin, tavg, and tmax 
# for your trip using the previous year's data for those same dates.

```


```python
# Plot the results from your previous query as a bar chart. 
# Use "Trip Avg Temp" as your Title
# Use the average temperature for the y value
# Use the peak-to-peak (tmax-tmin) value as the y error bar (yerr)

```


```python
# Calculate the rainfall per weather station for your trip dates using the previous year's matching dates.
# Sort this in descending order by precipitation amount and list the station, name, latitude, longitude, and elevation


```

## Optional Challenge Assignment


```python
# Create a query that will calculate the daily normals 
# (i.e. the averages for tmin, tmax, and tavg for all historic data matching a specific month and day)

def daily_normals(date):
    """Daily Normals.
    
    Args:
        date (str): A date string in the format '%m-%d'
        
    Returns:
        A list of tuples containing the daily normals, tmin, tavg, and tmax
    
    """
    
    sel = [func.min(Measurement.tobs), func.avg(Measurement.tobs), func.max(Measurement.tobs)]
    return session.query(*sel).filter(func.strftime("%m-%d", Measurement.date) == date).all()
    
daily_normals("01-01")
```


```python
# calculate the daily normals for your trip
# push each tuple of calculations into a list called `normals`

# Set the start and end date of the trip

# Use the start and end date to create a range of dates

# Stip off the year and save a list of %m-%d strings

# Loop through the list of %m-%d strings and calculate the normals for each date

```


```python
# Load the previous query results into a Pandas DataFrame and add the `trip_dates` range as the `date` index

```


```python
# Plot the daily normals as an area plot with `stacked=False`

```
