## Hackerone Profile Scraper

My :spaghetti: code to scrape all the profiles from hackerone.com

### Description

This python code has two functions:

* `get_users_handles()` will fetch all the handles of the **USERS** from `https://hackerone.com/sitemap/`.
 
* `crawl_user_profiles()` will fetch the stored handles and scrape the following data from `https://hackerone.com/{handle}`

| # | Name |
|---|---|
| 1 | id |
| 2 | identifier  |
| 3 | username  |
| 4 | name  |
| 5 | intro_html  |
| 6 | reputation  |
| 7 | signal_  |
| 8 | impact  |
| 9 | signal_percentile  |
| 10 | impact_percentile  |
| 11 | rank  |
| 12 | created_at  |
| 13 | location  |
| 14 | website  |
| 15 | bio  |
| 16 | profile_picture  |
| 17 | bugcrowd_handle  |
| 18 | github_handle  |
| 19 | gitlab_handle  |
| 20 | linkedin_handle  |
| 21 | twitter_handle  |
| 22 | resolved_report_count  |

* both functions will store the data in MySQL database in two separted tables

| Tables |
| --- |
| handles |
| profiles |


### Note

* Create the **handles** and **profiles** tables:
```sql
CREATE TABLE `handles` (
  `id` int(11) NOT NULL,
  `resource_identifier` varchar(800) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

CREATE TABLE `profiles` (
  `id` int(11) NOT NULL,
  `identifier` varchar(800) DEFAULT NULL,
  `username` varchar(800) DEFAULT NULL,
  `name` varchar(800) DEFAULT NULL,
  `intro_html` varchar(800) DEFAULT NULL,
  `reputation` varchar(800) DEFAULT NULL,
  `signal_` varchar(800) DEFAULT NULL,
  `impact` varchar(800) DEFAULT NULL,
  `signal_percentile` varchar(800) DEFAULT NULL,
  `impact_percentile` varchar(800) DEFAULT NULL,
  `rank` varchar(800) DEFAULT NULL,
  `created_at` varchar(800) DEFAULT NULL,
  `location` varchar(800) DEFAULT NULL,
  `website` varchar(800) DEFAULT NULL,
  `bio` varchar(800) DEFAULT NULL,
  `profile_picture` varchar(800) DEFAULT NULL,
  `bugcrowd_handle` varchar(800) DEFAULT NULL,
  `github_handle` varchar(800) DEFAULT NULL,
  `gitlab_handle` varchar(800) DEFAULT NULL,
  `linkedin_handle` varchar(800) DEFAULT NULL,
  `twitter_handle` varchar(800) DEFAULT NULL,
  `resolved_report_count` varchar(800) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

```
* Change your Mysql server credentials in **config.py** before running the script.
* Install all the requirements `pip3 install -r requirements.txt`.

---

### Bonus :moneybag:

I crawled **~633,801** handles & **~12,483** profiles, you can run `crawl_user_profiles()` to finish with the rest.

You can find the crawled data in 3 different formats: **csv** - **json** - **sql**

```
data
├── handles
│   ├── csv
│   ├── json
│   └── sql
│   └── yml
└── profiles
    ├── csv
    ├── json
    └── sql
    └── yml
    
```

### Data Sample

* Handle

```
...
(1508, 'aaron_costello'),
...
```

* Profile

```
...
(171, 'Z2lkOi8vaGFja2Vyb25lL1VzZXIvMTY1ODg=', 'aaron_costello', 'Aaron Costello', '', '1265', '2.32195121951219', '10.0847457627119', '72', '76', '504', '2015-01-21T22:49:01.678Z', 'Ireland', NULL, '', 'https://profile-photos.hackerone-user-content.com/variants/mmjL6s1r8BZG6Trrs8fffxUf/692237eb9691fbde92a17174cdb809a788ace02bc77c72803f17d32b41e4f213', '', 'aaron-costello', '', 'aaron-costello-226858a7', 'Conspiracyproof', '74'),
...
```

### Why All that ?

Any piece of information can help you to do other good things, imagine you want to see all the websites of each user who has at least one or more resolved report ?

```sql 
SELECT username,website FROM `profiles` WHERE resolved_report_count > 0 AND website is not null ;
```

![data](https://user-images.githubusercontent.com/43368124/74335899-d7948980-4da5-11ea-8186-c879bee1c5ed.png)


Imagine you want to count the number of the users who have > 0 resolved report UNION users with the rank between 100 AND 500
UNION users who have reputation between 10 AND 150

```sql
SELECT COUNT(*) as coun FROM `profiles` WHERE resolved_report_count > 0 
UNION
SELECT COUNT(*) FROM `profiles` WHERE rank BETWEEN 100 AND 500
UNION 
SELECT COUNT(*) FROM `profiles` WHERE reputation BETWEEN 10 AND 150

```
| condition | count() |
|---|---|
| resolved | 724 |
| rank between 100 AND 500 | 51 |
| reputation between 10 AND 150 | 1564 |

![chart (4)](https://user-images.githubusercontent.com/43368124/74353015-f86ad800-4dc1-11ea-920c-04549a89866c.png)



