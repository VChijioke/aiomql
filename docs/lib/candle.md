# Candle and Candles
Candle and Candles classes for handling bars from the MetaTrader 5 terminal.

## Table of Contents
- [Candle](#candle.candle)
  - [\_\_init\_\_](#candle.__init__)
  - [set_attributes](#candle.set_attributes)
  - [is_bullish](#candle.is_bullish)
  - [is_bearish](#candle.is_bearish)
  - [dict](#candle.dict)
- [Candles](#candles.candles)
  - [\_\_init\_\_](#candles.__init__)
  - [ta](#candles.ta)
  - [ta_lib](#candles.ta_lib)
  - [data](#candles.data)
  - [rename](#candles.rename)
  - [plot](#candles.plot)
  - [make_subplot](#candles.make_subplot)

<a id="candle.candle"></a>
### Candle
```python
class Candle
```
A class representing bars from the MetaTrader 5 terminal as a customized class analogous to Japanese Candlesticks.
You can subclass this class for added customization.

### Attributes

| Name          | Type        | Description                                                             |
|---------------|-------------|-------------------------------------------------------------------------|
| `time`        | `int`       | Period start time                                                       |
| `open`        | `int`       | Open price                                                              |
| `high`        | `float`     | The highest price of the period                                         |
| `low`         | `float`     | The lowest price of the period                                          |
| `close`       | `float`     | Close price                                                             |
| `tick_volume` | `float`     | Tick volume                                                             |
| `real_volume` | `float`     | Trade volume                                                            |
| `spread`      | `float`     | Spread                                                                  |
| `Index`       | `int`       | Custom attribute representing the position of the candle in a sequence. |
| `index`       | `Timestamp` | Index of the object in the DataFrame, as a timestamp.                   |

<a id='candle.__init__'></a>
### \_\_init\_\_
```python
def __init__(**kwargs)
```
Create a Candle object from keyword arguments. Kwargs are set as instance attributes. Open, high, low, close must be
provided during each instantiation.
#### Parameters: 
| Name     | Type  | Description                                        |
|----------|-------|----------------------------------------------------|
| `kwargs` | `Any` | Candle attributes and values as keyword arguments. |

#### Raises:
| Exception    | Description                                   |
|--------------|-----------------------------------------------|
| `ValueError` | If open, high, low, or close is not provided. |

<a id="candle.set_attributes"></a>
### set\_attributes
```python
def set_attributes(**kwargs)
```
Set keyword arguments as instance attributes
#### Parameters:
| Name     | Type  | Description                                        |
|----------|-------|----------------------------------------------------|
| `kwargs` | `Any` | Candle attributes and values as keyword arguments. |

<a id="candle.is_bullish"></a>
### is_bullish
```python
def is_bullish() -> bool
```
A simple check to see if the candle is bullish.
#### Returns:
| Type   | Description   |
|--------|---------------|
| `bool` | True or False |

<a id="candle.is_bearish"></a>
### is_bearish
```python
def is_bearish() -> bool
```
A simple check to see if the candle is bearish.
#### Returns:
| Type | Description   |
|------|---------------|
| bool | True or False |

<a id="candle.dict"></a>
### dict
```python
def dict(self, exclude: set = None, include: set = None) -> Dict[str, Any]
```
Return a dictionary representation of the Candle object.
#### Parameters:
| Name      | Type       | Description                                         |
|-----------|------------|-----------------------------------------------------|
| `exclude` | `set[str]` | A set of attributes to exclude from the dictionary. |
| `include` | `set[str]` | A set of attributes to include in the dictionary.   |

#### Returns:
| Type             | Description                                       |
|------------------|---------------------------------------------------|
| `Dict[str, Any]` | A dictionary representation of the Candle object. |


### <a id="candles.candles"></a> Candles
```python
class Candles(Generic[_Candle])
```
An iterable container class of Candle objects in chronological order. It is in a way a wrapper around a Pandas DataFrame
object. All the data pulled from the chart is stored as a pandas DataFrame object. In an attribute called **data**.
This class can be sliced, iterated over, and indexed like a sequence. It also has access to the pandas_ta library.
Indexing it returns a Candle object. It can be sliced to return a new instance of the class with the sliced candles.
This slices and resets the index of the underlying dataframe object. Key based indexing is also supported on the candles
object for accessing the columns of the underlying data attribute.

### Attributes:
The attributes of this class vary depending on the columns of underlying **data** attribute. i.e. each column of the **data**
attribute is an attribute of the class.

| Name          | Type                  | Description                                                       |
|---------------|-----------------------|-------------------------------------------------------------------|
| `data`        | `DataFrame`           | The pandas DataFrame containing the data.                         |
| `Index`       | `Series['int']`       | A pandas Series of the indexes of all candles in the object       |
| `index`       | `Series['Timestamp']` | DatetimeIndex of the underlying DataFrame object.                 |
| `time`        | `Series['int']`       | A pandas Series of the time of all candles in the object          |
| `open`        | `Series[float]`       | A pandas Series of the opening price of all candles in the object |
| `high`        | `Series[float]`       | A pandas Series of the high price of all candles in the object    |
| `low`         | `Series[float]`       | A pandas Series of the low price of all candles in the object     |
| `close`       | `Series[float]`       | A pandas Series of the closing price of all candles in the object |
| `tick_volume` | `Series[float]`       | A pandas Series of the tick volume of all candles in the object   |
| `real_volume` | `Series[float]`       | A pandas Series of the real volume of all candles in the object   |
| `spread`      | `Series[float]`       | A pandas Series of the spread of all candles in the object        |
| `timeframe`   | `TimeFrame`           | The timeframe of the candles in the object                        |
| `Candle`      | `Type[Candle]`        | The Candle class for representing the candles in the object.      |
| `data`        | `DataFrame`           | A pandas DataFrame of all candles in the object.                  |

#### Notes
When subclassing this class, make sure the Candle attribute is set to your desired candle class.

<a id="candles.__init__"></a>
### \_\_init\_\_
```python
def __init__(*,
             data: DataFrame | _Candles | Iterable,
             flip=False,
             candle_class: Type[_Candle] = None)
```
A container class of Candle objects in chronological order.
#### Parameters:
| Name           | Type                                   | Description                                                         | Default |
|----------------|----------------------------------------|---------------------------------------------------------------------|---------|
| `data`         | `DataFrame` or `Candles` or `Iterable` | A pandas dataframe, a Candles object or any suitable iterable       |
| `flip`         | `bool`                                 | Reverse the chronological order of the candles to the oldest first. | False   |
| `candle_class` | `Type[Candle]`                         | A subclass of Candle to use as the candle class.                    | Candle  |

<a id="candles.ta"></a>
### ta
```python
@property
def ta()
```
Access to the pandas_ta library for performing technical analysis on the underlying data attribute. Use this as you
would use the pandas_ta library on a pandas DataFrame. For inplace operations. The underlying data attribute is modified.
#### Returns:
| Type        | Description           |
|-------------|-----------------------|
| `pandas_ta` | The pandas_ta library |

<a id="candles.ta_lib"></a>
### ta\_lib
```python
@property
def ta_lib()
```
Access to the ta library for performing technical analysis. Not dependent on the underlying data attribute. Use this for
functions that require pandas Series as input.
#### Returns:
| Type | Description    |
|------|----------------|
| ta   | The ta library |

<a id="candles.data"></a>
### data
```python
@property
def data() -> DataFrame
```
A pandas DataFrame of all candles in the object.

<a id="candles.rename"></a>
### rename
```python
def rename(inplace=True, **kwargs) -> _Candles | None
```
Rename columns of the data object.
#### Parameters:
| Name      | Type   | Description                                                                               | Default |
|-----------|--------|-------------------------------------------------------------------------------------------|---------|
| `inplace` | `bool` | Rename the columns inplace or return a new instance of the class with the renamed columns | True    |
| `kwargs`  | `str`  | The new names of the columns                                                              |         |

#### Returns:
| Type      | Description                                                               |
|-----------|---------------------------------------------------------------------------|
| `Candles` | A new instance of the class with the renamed columns if inplace is False. |


<a id="candles.plot"></a>
### plot
```python
def plot(subplots=None, span: int = None, filename="", **kwargs):
```
Create a plot with mplfinance, can be saved as png.

#### Parameters:
| Name       | Type   | Description             | Default |
|------------|--------|-------------------------|---------|
| `subplots` | `dict` | Add subplots            | None    |
| `span`     | `int`  | Take the last n candles | None    |
| `filename` | `str`  | A name to save plot     | ""      |
| `**kwargs` | `Any`  | Kwargs to plot function |         |


<a id="candles.make_subplot"></a>
### make_subplot
```python
def make_subplot(column: str | list[str], span: int = None, **kwargs):
```
Create a plot with mplfinance, can be saved as png.

#### Parameters:
| Name       | Type              | Description                                      | Default |
|------------|-------------------|--------------------------------------------------|---------|
| `column`   | `list[str]\| str` | An iterable of column names as a list or strings | None    |
| `span`     | `int`             | Take the last n candles                          | None    |
| `**kwargs` | `Any`             | Kwargs to plot function                          |         |
