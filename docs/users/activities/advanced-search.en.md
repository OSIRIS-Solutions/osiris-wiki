---
status: false
tags:
    - activities
---

# Advanced activity search

[:simple-youtube: To the video](https://youtu.be/cKuugdRF-k8){.md-button}

You can search the **All activities** page quickly and easily using the search bar at the top left. If a simple search is not enough, you can perform an **advanced search**, which you will find directly next to the search bar.


## Search with operators

The advanced search in OSIRIS offers you a variety of options for filtering the entries in the database. You can add further criteria using the **New rule** button. You can use the **AND/OR** buttons above the category field to determine the dependency of your search fields. If you would like to use a combination of AND/OR, you can add further fields and create a new dependency using the **new group** button at the top right.

![Example advanced search](screenshots/advancedsearch_bsp1.png)
///caption
**Example of an advanced search:**: This search will show you all activities that are a Journal Article AND have an Impact Factor greater than 2 AND were published in 2025 OR 2024.
///

You can remove any search field from your search using the *delete* button to the right of it.

You can use the **Select columns** function to add or remove as many columns as you like to the results table of your search. If you click on the button, a widget opens which shows you all available columns and which you can select with a click. Columns that are not available are greyed out.

![Add column widget](screenshots/columns_add.png)
///caption
The add column widget allows you to add any columns to your results table.
///

![Add column result](screenshots/columns_add_table.png)
///caption
This is what your table looks like when you have added the columns "Journal" and "Impact Factor" in the widget.
///

## Save search queries

You can save your current search with the **Saved queries** button and use it again and again. A widget opens, which displays previously saved searches and allows you to give your current search a name. After you have saved the search, you must reload the page before you can find your search again in the saved queries.

![Save widget search](screenshots/widget_search_save.png)
///caption
In the "Saved queries" widget, you can see all your saved searches and can give your current search a name. By clicking on a saved search, you can view the information and delete the search.
///

## Aggregate a search

Next to the "Select column" function you will find the **Aggregate** button. You can use this function to create an overview table of the number of entries in your results table. If you aggregate by year, for example, the table will show you the number of journal articles per year. The aggregation table depends on your current search.

![Aggro table](screenshots/aggregation_table.png)
///caption
Here you aggregate the search shown above by "Year". The table shows you the number of entries per year.
///


## Copy search and continue using it

Click on the **Show filter** button to display the MongoDB code for your filter. You can use this code to share your search with others or to reuse it yourself. For example, you can use the code to filter the activities in the **Report template** function. Here, for example, you can see the code for the search performed above.

```bash
{"$and":[{"subtype":"article"},{"impact":{"$gt":2}},{"$or":[{"year":2025},{"year":2024}]}]}
```

## Expert mode

Next to the orange "Apply" button you will find the *Expert mode*. Here you can enter your own MongoDB commands to search OSIRIS.