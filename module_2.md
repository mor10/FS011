## Using `.assign()`

### Expanded example

```python
lego_details = (washed_lego.assign(
    total_pieces = washed_lego['matte'] + washed_lego['transparent']
    )
    .sort_values('total_pieces', ascending=False)   
               )
```