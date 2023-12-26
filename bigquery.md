# BigQuery Tips

* Best practice is to always start with the largest table, followed by the smallest and then by a decreasing size.

## Interesting links

* https://cloud.google.com/bigquery/docs/best-practices-performance-compute
* https://kingfluence.com/display/DT/How+to+write+optimal+SQL+in+BigQuery#
* https://towardsdatascience.com/10-top-tips-unleash-your-bigquery-superpowers-c87488621c71

## Unnesting tables

This is where the UNNEST function comes in. It basically lets you take elements in an array and expand each one of these individual elements. You can then join your original row against each unnested element to add them to your table.

In this example:
![](https://miro.medium.com/max/1416/1*Wx8OSBUETOVmCO7zn8Hmrw.png)

We can't do a:

SELECT * FROM `spaceships` WHERE crew = "Zoe"

But should unnest and cross join

SELECT * FROM `spaceships` CROSS JOIN UNNEST(crew) as crew_member

What BigQuery will do is take every individual member in my crew array, and add it on as a new column in my row called crew_member It will repeat my original row as needed to accompany each new value.

And look up for Zoe:

SELECT * FROM `spaceships` 
CROSS JOIN UNNEST(crew) as crew_member 
WHERE crew_member = "Zoe"

most BigQuery developers will replace the CROSS JOIN with a comma, like so:

SELECT * FROM `spaceships`,
UNNEST(crew) as crew_member 
WHERE crew_member = "Zoe"

One may wonder why the CROSS JOIN does not expand all the rows as a regular CROSS JOIN aming two unrelated tables. A good explanation here:
<https://stackoverflow.com/questions/47670451/sql-array-flattening-why-doesnt-cross-join-unnest-join-every-nested-value-with>.

## Tips

* How to give read permissions to everyone on a project: datacommons.prod.all.readers@king has "BigQuery Data Viewer" permission.
