
# BestPractices

Some **best practices** categorized by app development platform we are using at Jamit Labs.

The goal of this project is both to **provide many articles to cover independent topics** using a typical blogpost-like length and to provide a book-like structure meaning that **articles are sorted to reflect their topics relevance** regarding the stage of the projects development and/or the programmers learning progress.

This has two main advantages:

- articles can be read **on-demand** (meaning: when they are needed) and work well as **reference**
- beginners can **acquire knowledge step by step** by following the articles structure

## Contributing

The following are instructions on how to contribute to this project.

### Requirements

- Ruby 2.2+

You can easily install a compatible version of Ruby by running the following commands in terminal (Mac):

```shell
# install RVM, a Ruby version manager:
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ \curl -sSL https://get.rvm.io | bash -s stable

# install Ruby 2.2:
$ rvm install ruby 2.2.3
```

### Starting the server locally

Before you can start a Jekyll server locally you need to install the dependencies once by running the following:

```shell
$ gem install bundler
$ bundle install
```

Now you can **start the server locally** by running:

```shell
$ jekyll serve
```

As long as the server runs you can **visit the page in your browser** by opening [localhost:4000](http://localhost:4000) or [127.0.0.1:4000](http://127.0.0.1:4000).
To **stop the local server** in your terminal type `Ctrl+C` on your keyboard.


### Creating a new article

Before creating a new article take a look at the existing articles and topics to **decide where the best place** is for your new article. Once decided **create an empty article file** within the appropriate folder. If your articles belongs to a topic that was not yet covered by any of the existing folders then **create a new topic folder**.

**Stick to the following rules** when creating new files:

**NEW TOPIC RULES**

- A topic folder must be created within a category folder
- Category folders are placed within the folder `_articles`, currently: `General`, `Apple` and `Android`
- A topic folder is named using the structure `<CATEGORY_ID><TOPIC_ID> <TOPIC_NAME>`
- The `<CATEGORY_ID>` is `GN` for the category `General`, `AP` for `Apple` and `AN` for `Android`
- A `<TOPIC_ID>` is a three digit number starting at `010` for the first topic within a category
- A new topic that is appended at the end of the current topics increases the highest id by 10
- A new topic that belongs between existing topics uses the average of the two neighbors topic ids
- The `<TOPIC_NAME>` is always written in English

**NEW ARTICLE RULES**

- A new article must be created within at topic folder
- A new article is named using the structure `<ARTICLE_ID> <ARTICLE_NAME>.<LANGUAGE_CODE>.md`
- An `<ARTICLE_ID>` is a four digit number starting at `0100` for the first article within a topic
- A new article that is appended at the end of the current articles increases the highest id by 100
- A new article that belongs between existing articles uses the average of the two neighbors topic ids
- The `<ARTICLE_NAME>` is always written in English
- The `<LANGUAGE_CODE>` is `de` for articles written in German and `en` for articles written in English

#### Using images in new articles

Follow these steps to use images within new articles:

- Create a folder named `<ARTICLE_ID>` within `public/images/<TOPIC_ID>`
- Place your images into this new folder
- Use lower letter characters and replace whitespaces and special characters with `-`
- Write your image file names in English
- Include your image using `![<IMAGE_TITLE>](../../../public/images/<TOPIC_ID>/<ARTICLE_ID>/<IMAGE_NAME>.png)`
- Make sure the `<IMAGE_TITLE>` describes the content of the image in short (mostly 2-5 words)
- Make sure to write a comment that explains your image to the reader (redundancy is a good thing here).
- Place the comment in the line below the image inclusion using the structure `*<IMAGE_COMMENT>*`

## License

<p style="float: right;">
    <img src="by-nc-sa.eu.png"
      width=143 height=50>
</p>

This work is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License**. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/ or open the file `LICENSE.txt`.
