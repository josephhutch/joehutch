---
title: "Aether: A Clean Theme for Hugo"
date: 2018-05-02T14:33:42-04:00
description: "Aether is new theme for Hugo (a static site generator) that emphasizes motion, material, and depth as design elements. Distracting styling and page elements are forgone to focus on the content."
categories: ["Web-Dev"]
dropCap: true
displayInMenu: false
displayInList: true
draft: false
resources:
- name: featuredImage
  src: "mdd-macbook.jpg"
  params:
    description: "A MacBook showing a website using the aether theme"
---

Today's web is a frustrating mess of pop-ups, intrusive banners, and ads obstructing the content. I designed aether to be free of all these distractions and simply highlight the content. The result feels more like a native application than a website. Interactions are intuitive, content is the focus, and distractions are omitted.

Aether is a Hugo theme for blogs that elevates good writing and photography. If you use Hugo for your blog, give aether a try.

## Installation
In the root directory of your Hugo Project, clone the aether repo into the themes directory.

```shell session
git clone https://github.com/josephhutch/aether.git themes/aether
```

## Usage

### Website Configuration

Add the following parameters to your config file if they are not present already.

```toml
baseURL = "https://yourwebsitenamegoeshere.com"
languageCode = "your language code"
title = "your website title"
theme = "aether"
googleAnalytics = "Your google analytics tracking ID"

[params]
brand = "the title displayed in the nav bar - optional"
description = "your website's description"
homeimg = "url to the image used for the home button - optional"
bgimg = "url to the image used for the background - optional"
```

The `title` parameter is used for each page title, the title that seach engines display in search results. If you would like the title shown in the top left of the page to be different from the page title, use the `brand` parameter. For instance, the title parameter for my site is `Joe Hutchinson` but the brand parameter is set to `joehutch`.

The `homeimg` and `bgimg` parameters give you the ability to customize the look of your site further. The homeimg parameter is the image used for the home button at the bottom of every page. Since the text used on the home button is white, a darker background image is preferred. If the homeimg parameter is not specified, a fallback image is used. Similarly, the bgimg parameter is used for the background of each webpage. Aether is designed to look best with a subtle tiling image for the background. If no background image is specified, the background will be a solid gray color.

That is the only configuration required at the site level! You can now begin writing content for your site.

{{<image src="post/aether-theme/mdd-iphone.jpg" alt="aether theme displayed on an iPhone" >}}

### Creating content
Make a new blog post by executing `hugo new post/postnamehere.md` in your shell. At the top of the new markdown file, is what's called the front matter. The front matter is the page's metadata that determines how Hugo and aether generate the HTML for your post. Below you can find what the front matter should contain for a new post and what each of the parameters mean.

```properties
---
title: "The title of your post"
date: date the post was generated
description: "Description of the post (displayed in the post's card)"
categories: ["add comma separated categories here", "another category"]
dropCap: if the first letter should be a large decorative capital letter (true, false)
displayInMenu: if post is listed in the navigation menu (true, false)
displayInList: if post is listed on the home page and category pages (true, false)
draft: if the page is a draft (true, false)
resources:
- name: featuredImage
  src: "URL to the page's featured image, used as the card image and the image at the top of the article"
  params:
    description: "Description for the featured image, used as the alt text"
---
---
```

The `displayInMenu` and `displayInList` parameters are used to determine where your content is displayed. Posts typically have displayInMenu set to false so that the post is not a menu option, and displayInList set to true so it shows up on the homepage's list of posts and in category page lists. An About Me page, on the other hand, would have displayInMenu set to true and displayInList set to false.  That will allow the About Me page to be accessible from the menu but not displayed in the homepage's list of posts.

The `categories` parameter is used to group similar posts in category pages. Category pages are accessible from the menu and list all posts with the same category.

The `dropCap` parameter is used to determine if the first letter of a post should be a dropped capital. A dropped capital letter is the large decorative letter at the beginning of a book or section.

Add an interesting description and a good image to each post to get the most value from this theme.

Posts are written in markdown and LaTeX (for math symbols and equations). You can find tons of information on how to format your posts with markdown and LaTeX on the web. Below are examples of how markdown and LaTeX looks when generated into HTML with aether. For each example below, first is the markdown and the second is how the resulting HTML looks. This is not an exhaustive list.

#### LaTeX style math typesetting with KaTeX

```md
{{</* raw */>}}
\[u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d\tau + K_d \frac{de(t)}{dt} \]
{{</* /raw */>}}
```

{{< raw >}}
\[u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d\tau + K_d \frac{de(t)}{dt} \]
{{< /raw >}}

#### Code (Supports many programming languages and formats)

````md
```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```
````

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

#### Inline code

```md
Here is `var s = "Hello World"` inline code
```

Here is `var s = "Hello World"` inline code

#### Tables

```md
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

#### Ordered List

```md
1. Number one
2. Number Two
    1. Indented Number 1
    2. Indented Number 2
```

1. Number one
2. Number Two
    1. Indented Number 1
    2. Indented Number 2

#### Unordered List

```md
* Get groceries at Harris Teeter before the party
* Get a Spider Man cake
    * Chocolate or marble
    * Whipped cream frosting
* Don't forget to walk the dog before you leave
* Bring lots of plates and silverware so that we don't run out
    * Plastic Dixie brand is fine
```

* Get groceries at Harris Teeter before the party
* Get a Spider Man cake
    * Chocolate or marble
    * Whipped cream frosting
* Don't forget to walk the dog before you leave
* Bring lots of plates and silverware so that we don't run out
    * Plastic Dixie brand is fine

#### Comments

```md
> This is some text that should show up as a comment. Someone may have made this comment but i'm not sure.
```

> This is some text that should show up as a comment. Someone may have made this comment but i'm not sure.

#### Images

```md
![aether theme displayed on an iPhone](post/aether-theme/mdd-iphone.jpg)

<!--- OR with webp support ... -->

{{</* image src="post/aether-theme/mdd-iphone.jpg" alt="aether theme displayed on an iPhone" */>}}
```

{{<image src="post/aether-theme/mdd-iphone.jpg" alt="aether theme displayed on an iPhone" >}}

#### Small Images

```md
{{</* smallimg src="post/aether-theme/mdd-iphone.jpg" alt="aether theme displayed on an iPhone" smartfloat="left" width="250px" */>}}
```

{{<smallimg src="post/aether-theme/mdd-iphone.jpg" alt="aether theme displayed on an iPhone" smartfloat="left" width="250px">}}

This image floats to the left of this paragraph and is 250px wide. Its aspect ratio is maintained so it will not stretch. The picture shows aether on an iPhone. You can see how the design is responsive and how the cards intelligently fit to the display. With flexbox and css grid, heavy frameworks such as bootstrap aren't necessary to create beautiful responsive designs. The cards in aether use flexbox to change the image from the right side on desktops to the top on mobile.

#### Links

```md
[Aether's Github page](https://github.com/josephhutch/aether)
```

[Aether's Github page](https://github.com/josephhutch/aether)

### Further Customization
To change the heading and subtext at the top of list pages just add a \_index.md file in the folder that the list page is generated from. For example to change the heading at the top of the homepage, add an \_index.md file to the content folder with the following parameters.

```properties
---
title: "This is the main heading text in big letters"
date: the date
description: "This is the subtext above the main heading in small letters"
---
```

## Learn  More

To learn more about aether, visit [aether's Github page](https://github.com/josephhutch/aether). To learn more about Hugo, visit [Hugo's website](https://gohugo.io/).