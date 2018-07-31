

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




    {'_sa_instance_state': <sqlalchemy.orm.state.InstanceState at 0x1eff74c4be0>,
     'date': '2010-01-01',
     'id': 1,
     'prcp': 0.08,
     'station': 'USC00519397',
     'tobs': 65.0}




```python
first_row = session.query(Station).first()
first_row.__dict__
```




    {'_sa_instance_state': <sqlalchemy.orm.state.InstanceState at 0x1eff75045c0>,
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
plt.xticks(np.arange(0, 0.05, step=0.5))
plt.legend(loc="upper left")

plt.show()

plt.savefig("hawaii_vacay.png",bbox_inches="tight")
```


    <IPython.core.display.Javascript object>



<img src="data:image/png;base64,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" width="800">



```python


x_axis = len(climate_df1)
tick_locations = [value for value in x_axis]
plt.hist(x_axis, climate_df1["Precipitation"], color='b', alpha=0.5, align="center")
plt.figure(figsize=(20,3))
plt.xticks(tick_locations, climate_df1["Precipitation"], rotation="vertical")

multi_plot.set_xticklabels(climate_df1["Precipitation"], rotation=45)
plt.tight_layout()
plt.grid()
plt.show()
```


```python
# Use Pandas to calcualte the summary statistics for the precipitation data

```


```python
# How many stations are available in this dataset?

```


```python
# What are the most active stations?
# List the stations and the counts in descending order.

```


```python
# Using the station id from the previous query, calculate the lowest temperature recorded, 
# highest temperature recorded, and average temperature most active station?

```


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
