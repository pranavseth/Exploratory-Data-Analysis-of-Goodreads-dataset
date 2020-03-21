```python
import pandas as pd
import seaborn as sns
```

```python
data=pd.read_csv('books.csv',error_bad_lines=False,parse_dates=True)
data.rename(columns={"  num_pages":"num_pages"},inplace=True)
data.info()
```

There are 12 features in the dataset, as follows.
1.	bookID: A unique
Identification number for each book.
2.	title: The name under which the book was
published.
3.	authors: Names of the authors of the book. Multiple authors are
delimited with -.
4.	average_rating: The average rating of the book received in
total.
5.	isbn: Another unique number to identify the book, the International
Standard Book Number.
6.	isbn13: A 13-digit ISBN to identify the book, instead
of the standard 11-digit ISBN.
7.	language_code: Helps understand what is the
primary language of the book. For instance, eng is standard for English.
8.
num_pages: Number of pages the book contains.
9.	ratings_count: Total number of
ratings the book received.
10.	text_reviews_count: Total number of written text
reviews the book received.
11.	publication_date: Date when the book was first
published.
12.	publisher: The name of the publisher.

```python
data=data[data.ratings_count>100]
data.info()
```

This excludes books with authors "NOT A BOOK", average ratings of 0.00 and books
with low number of ratings not relevant enough to be considered.

```python
data.authors.str.count("/").sort_values(ascending=False).head(5)
```

There are books with over 30 authors. Must be a collection of short stories.

```python
data.groupby('language_code',as_index=False)['title'].count().sort_values('title',ascending=False).head(15).set_index('language_code').plot.bar().set_xlabel("Languages")
```

English and it's dialects are the most popular language.

```python
most_rated = data.sort_values('ratings_count',ascending=False).head(15).set_index('title')
plt = most_rated['ratings_count'].plot.barh()
plt.set_xlabel("Total ratings count ")
plt.set_ylabel("Books")
plt.set_title("Top 15 most rated books")
```

The first book of the Twilight series is the most rated, by a country mile.

```python
sns.jointplot(x="num_pages",y="average_rating",data=data[data.num_pages<2000])
```

More the number of pages, higher the probability of a higher average rating.

```python
check2=data.groupby('publisher')['average_rating'].agg(['count','mean']).reset_index()
check2=check2.sort_values('count',ascending=False).head(20)
check2=check2[['publisher','mean']]
check2=check2.set_index('publisher')
plt=check2.plot.barh()
```

Vintage is the publisher with most publications while VIZ Media LLC is the
publication with highest average rating of publications.

```python
check3=data.groupby('publisher')['num_pages'].agg(['count','mean']).reset_index()
check3=check3.sort_values('count',ascending=False).head(20)
check3=check3[['publisher','mean']]
check3=check3.set_index('publisher')
plt=check3.plot.barh()
```

VIZ Media LLC has most publications with the highest average rating and least
number of pages. Efficient.

```python
check4=data.groupby('authors')['average_rating'].agg(['count','mean']).reset_index()
check4=check4.sort_values('count',ascending=False).head(10)
check4=check4[['authors','mean']]
check4=check4.set_index('authors')
plt=check4.plot.barh()
```

Stephen King is the most published author while P.G. Wodehouse is a close
second. P.G. Wodehouse is also the author with highest mean average rating.

```python
most_pages=data.sort_values('num_pages',ascending=False).head(15).set_index('title')
most_pages['num_pages'].plot.barh()
```

With 5 books, The Complete Aubrey/Maturin Novels has over 6000 pages while The
Second World War is the single most book with about 5000 pages.
