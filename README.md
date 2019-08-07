# Analysis of PyPI downloads by distro

From [BigQuery query](https://bigquery.cloud.google.com/savedquery/411151289586:9ceba9940d5e429da113851b07eb38f7):

```{sql}
#standardSQL
SELECT COUNT(*) AS downloads,
REGEXP_EXTRACT(details.python, r"[0-9]+\.[0-9]+") AS python_version,
details.distro.name as distro_name,
details.distro.version as distro_version
FROM `the-psf.pypi.downloads*`
WHERE file.project = 'numpy'
  AND file.type = 'bdist_wheel'
  -- Only query the last 30 days of history
  AND _TABLE_SUFFIX
    BETWEEN FORMAT_DATE(
      '%Y%m%d', DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY))
    AND FORMAT_DATE('%Y%m%d', CURRENT_DATE())
 GROUP BY python_version, distro_name, distro_version
 ORDER BY downloads DESC
```

Last updated 7 August 2019.
