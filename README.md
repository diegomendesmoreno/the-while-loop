![RFP Read Memory](static/assets/img/logo_whileloop_light.png)

# The While Loop

This is a tech blog filled with programming and embedded content.


### File Naming convention

There may be many files related to the same content (two articles in each language and the many images), so, for logistic purpuses, I chose to use a CRC-32 hash prefix generated from the name of the article file using [this online has tool](https://emn178.github.io/online-tools/crc32.html). See how it is done:

```
name_of_the_file >>>> Hash >>>> 0575d749
```

So, the names of the file become:
```
content/posts/0575d749_name_of_the_file.en.md
content/posts/0575d749_name_of_the_file.pt-br.md
static/assets/img/0575d749_image_1.png
static/assets/img/0575d749_image_2.png
static/assets/img/0575d749_image_3.png
```


### About

This site was made with [Hugo](https://gohugo.io/), a popular open-source static site generator using the theme [hello-friend-ng](https://themes.gohugo.io/themes/hugo-theme-hello-friend-ng/) made by Djordje Atlialp.
