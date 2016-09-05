
# BestPractices

Some **best practices** categorized by app development platform we are using at Jamit Labs.

The goal of this project is both to **provide many articles to cover independent topics** using a typical blogpost-like
length and to provide a book-like structure meaning that **articles are sorted to reflect their topics relevance**
regarding the stage of the projects development and/or the programmers learning progress.

This has two main advantages:

- articles can be read **on-demand** (meaning: when they are needed) and work well as **reference**
- beginners can **acquire knowledge step by step** by following the articles structure

## Contributing

The following are instructions on how to contribute to this project.

### Requirements

- Ruby 2.2+
- GPG (`brew install gpg` via Homebrew)

You can easily install a compatible version of Ruby by running the following commands in terminal (Mac):

```shell
# install RVM, a Ruby version manager:
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ \curl -sSL https://get.rvm.io | bash -s stable

# install Ruby 2.2:
$ rvm install ruby 2.2
```

If you have **trouble with Homebrew** while installing Ruby like suggested above, then try running `brew doctor` and
fixing all issues with the suggested changes that will then be presented to you. When finished with all suggested
changes don't forget to **restart your terminal** as some changes will only take effect after a restart. Then installing
ruby should work fine.

### Starting the server locally

Before you can start a Jekyll server locally you need to install the dependencies once by running the following:

```shell
$ gem install bundler
$ bundle install
```

Now you can **start the server locally** by running:

```shell
$ jekyll s
```

As long as the server runs you can **visit the page in your browser** by opening [localhost:4000/de](http://localhost:4000/BestPractices/de)
or [127.0.0.1:4000/de](http://127.0.0.1:4000/BestPractices/de).
To **stop the local server** in your terminal type `Ctrl+C` on your keyboard.


### Creating a new article

Before creating a new article take a look at the existing articles and topics to **decide where the best place** is for
your new article. Once decided **create an empty article file** within the appropriate folder. If your articles belongs
to a topic that was not yet covered by any of the existing folders then **create a new topic folder**.

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
- A new `<TOPIC_ID>` must be manually placed into the right section within the `_config.yml` file
- New translations must be added to the files under `_data/lang` for the `<TOPIC_ID>` keys in the `topic:` section

**NEW ARTICLE RULES**

- A new article must be created within a topic folder
- A new article is named using the structure `<ARTICLE_ID> <ARTICLE_NAME>.<LANGUAGE_CODE>.md`
- An `<ARTICLE_ID>` is a four digit number starting at `0100` for the first article within a topic
- A new article that is appended at the end of the current articles increases the highest id by 100
- A new article that belongs between existing articles uses the average of the two neighbors topic ids
- The `<ARTICLE_NAME>` is always written in English
- The `<LANGUAGE_CODE>` is `de` for articles written in German and `en` for articles written in English

Also make sure new articles begin with the following structure:

```yaml
---
section:    <CATEGORY_NAME>
topicid:    <TOPIC_ID>
refid:      <CATEGORY_ID><TOPIC_ID>-<ARTICLE_ID>
permalink:  /:language/articles/<REFID>.html
title:      <ARTICLE_NAME>
date:       <FIRST_PUBLISH_DATE>
author:     <MAIN_AUTHOR_NAME>
language:   <LANGUAGE_CODE>

prev_refid: <PREVIOUS_ARTICLE_REFID>
next_refid: <NEXT_ARTICLE_REFID>
---
```

Note that you should use `00:00:00` as the specified time in the field `FIRST_PUBLISH_DATE` as only the day is relevant to us. Also, the publish date refers to the initial GitHub release of the article and therefore can be set to anything in the past (e.g. `2016-01-01 00:00:00`) for new articles until release.

Make sure to remove `prev_refid` and/or `next_refid` if the article is the last/first/only one in its category. Please also **don't forget to update the `prev_refid` of the next article and the `next_refid` of the previous article** to point to your new article.

#### Using images in new articles

Follow these steps to use images within new articles:

- Create a folder named `<ARTICLE_ID>` within `public/images/<TOPIC_ID>`
- Place your images into this new folder
- Use lower letter characters and replace whitespaces and special characters with `-`
- Write your image file names in English
- Include your image using
`![<IMAGE_TITLE>](../../../BestPractices/public/images/<TOPIC_ID>/<ARTICLE_ID>/<IMAGE_NAME>.png)`
- Make sure the `<IMAGE_TITLE>` describes the content of the image in short (mostly 2-5 words)
- Make sure to write a comment that explains your image to the reader (redundancy is a good thing here).
- Place the comment in the line below the image inclusion using the structure `*<IMAGE_COMMENT>*`

#### Referencing other articles

To create a reference to another article use `refid` field of the references article with the following structure:

```markdown
[<refid>](<refid>.html)
```

### Contribution Tips

- Use the [Atom Editor](https://atom.io) and edit with a **live MarkDown preview** using the `Ctrl+Shift+M` shortcut
- Access all settings and features of Atom and its packages via the shortcut `Cmd+Shift+P` and fuzzy search
  - e.g. `Cmd+Shift+P`, then typing "install pack" and then hitting enter opens up the install packages screen
- Install the Atom package `line-length-break` and reformat your MarkDown file before committing
  - reformatting can be done within a file by hitting `Cmd+Shift+P` and searching for "break"

Note that the Atom Editor will currently fail to show the images due to the subpath 'BestPractices' in the image paths
(it isn't a real path but needed due to the current way of deployment). If you want to see the images in Atom simply
remove the '/BestPractices' from image paths but don't forget to add them before committing. Because of this it's
probably better to just run the server and open the browser instead.

## License

<p style="float: right;">
    <img src="by-nc-sa.eu.png"
      width=143 height=50>
</p>

This work is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License**. To
view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/ or open the file `LICENSE.txt`.
