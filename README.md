# Finance-Project

## Reading and writing Pickle
```python
import pickle 

example_dict = {'first': 23 , 'second': 45, 'third': 65, 'fourth': 'this will not be in the correct order' ,
'fifth': 'dictionaries are not ordered by number', 'sixth': 897}
example_dict['seventh'] = 'I did not do the keys correctly'
example_dict['eight'] = 9080

#print(example_dict.keys())
#print(example_dict.values())

# wb : write binary
pickle_out = open('dict.pickle', 'wb')

# read data from the dictionary and put in the pickle file
pickle.dump(example_dict, pickle_out)
pickle_out.close()

# rb : read bytes
pickle_in = open('dict.pickle', 'rb')
# read the pickle file and put information into the dictionary
example_dict = pickle.load(pickle_in)
```

## Reading and writing HDF5 format files

### seed for replication
```python

np.random.seed(123456)
# create a DataFrame of dates and random numbers in three columns
df = pd.DataFrame(np.random.randn(8, 3), 
                  index=pd.date_range('1/1/2000', periods=8),
                  columns=['A', 'B', 'C'])
```
### create HDF5 store
```python

store = pd.HDFStore('data/store.h5')
store['df'] = df # persisting happened here
store
```

```
<class 'pandas.io.pytables.HDFStore'>
File path: data/store.h5
/df            frame        (shape->[8,3])
```
### read in data from HDF5
```python

store = pd.HDFStore("data/store.h5")
df = store['df']
df[:5]
```

```
                   A         B         C
2000-01-01  0.469112 -0.282863 -1.509059
2000-01-02 -1.135632  1.212112 -0.173215
2000-01-03  0.119209 -1.044236 -0.861849
2000-01-04 -2.104569 -0.494929  1.071804
2000-01-05  0.721555 -0.706771 -1.039575
```

### this changes the DataFrame, but did not persist
```python

df.iloc[0].A = 1 
# to persist the change, assign the DataFrame to the 
# HDF5 store object
store['df'] = df
# it is now persisted
# the following loads the store and 
# shows the first two rows, demonstrating
# the the persisting was done
pd.HDFStore("data/store.h5")['df'][:5] # it's now in there
```
```
                   A         B         C
2000-01-01  1.000000 -0.282863 -1.509059
2000-01-02 -1.135632  1.212112 -0.173215
2000-01-03  0.119209 -1.044236 -0.861849
2000-01-04 -2.104569 -0.494929  1.071804
2000-01-05  0.721555 -0.706771 -1.039575
```
