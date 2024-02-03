## Using `.assign()`

### Expanded example

```python
lego_details = (washed_lego.assign(
                    total_pieces = washed_lego['matte'] + washed_lego['transparent']
                    )
                    .sort_values('total_pieces', ascending=False)   
               )
```

## Filtering columns

`high_protein_cereal = cereal[cereal['proteins']>4]`
`range_cereal = cereal[(cereal['protein']>=4) & (cereal['protein']<=5)]`
`high_protein_cereal = cereal[~(cereal['proteins']>4)]` // returns the compliment

## `.groupby()`

`largest_number = lego_tower.groupby(by='name')`

## `.sort_values()`

```python
largest_number = lego_tower.groupby(by='name')
                           .sum()
                           .sort_values('quantity', ascending=False)
```