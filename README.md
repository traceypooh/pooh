
# poohbot

poohbot.com static site version

## helpful related links
- https://fontawesome.com/v4.7.0/icons/

## To Do - criticals
- top nav
gg albums/images
gg albums/thumbs

- photos.md
- europe.md
```bash
gg -i poohbot.com
```
- aliases /img => /images    (for prior site 404s...)
- **aliases** every prior post url to current (when differ)
- **crawl poohBot.com**
```bash
# get log of all pages/assets findable, as well as 404s
wget --domains poohbot.com --recursive --page-requisites --spider --no-directories --no-verbose https://poohbot.com 2>&1 |tee poohbot.com

#
egrep -io '//poohbot.com[^ :]+' poohbot.com|fgrep -v poohbot.com/wp-|egrep -v '/feed/$'|sort -u -o chex

# now check urls from WP (and their includes) and ensure they're on GL now..
( for i in $(cat ~/poohbot/tmp/chex); do line; echo $i; wget --domains poohbot.com --page-requisites --no-verbose http:$i; done ) 2>&1 |tee frog


```
  - compare with crawl of hugo
    - ensure on poohBot.com / not hugo explainable or alias..
- crawl new site and fix all 404s
- /category/ => /categories/ alias
- /tag/      => /tags        alias


## To Do - nice to have
content/contact/_index.md
fgrep '<?' $(finddot md)
- `static/js/` location (re: production and post-DNS cutover)
```
gg lacer.php
gg kml.php
gg wp-
gg featured-click
```
- unbug users!
- /video/ and /lapses/ - switch to CSS grid for centering filmstrip
- /video/ and /lapses/ - 'Play all'
- pick 2 random posts for left side
- make videos take up full 854px wide (720x480 now)
- link any <img> local tag in a post to fullsize naked img?
- `embed: ` in .md auto does archive.org iframe..

### map route fails
```
gg https://poohbot.com/alc/morgan-territory/kml.kml
```
- http://localhost:1313/morgan-territory/
- http://localhost:1313/three-bears-and-mt-diablo/

### Imagery++
- http://localhost:1313/2015/05/slide-responsively-minimal-standalone-htm/css/js-inspired-by-sliding-door-from-wayne-connor/
- http://localhost:1313/2013/02/how-to-turn-time-machine-from-disk-with-many-partitions-to-single-partition-logically-extending-time-machine-partition/
- http://localhost:1313/2013/02/simple-way-to-make-h.264-mp4-web-and-ios/mobile-playable-video-mp4-files-for-linux-and-macosx-using-ffmpeg/
- http://localhost:1313/2012/06/convert-yuvj420p-to-yuv420p-chrome-playable-mp4-video-eg-canon/nikon-video/
- http://localhost:1313/2012/01/natively-compiling-ffmpeg-mplayer-mencoder-on-macos-lion-with-x264/
- http://localhost:1313/2011/05/natively-compiling-ffmpeg-x264-mplayer-on-mac-with-builtin-x264-and-webm-encoding/
- http://localhost:1313/2009/09/ffmpeg-for-time-lapse-sets-of-images-and-even-archiving/
- http://localhost:1313/2009/04/ffmpeg-building-on-mac-intel/x386/

### CSP
- gg -i onclick
- gg -i '<script>' |chopper
- gg '<style[^U]' |chopper |fgrep '<style'
- gg 'style='

---

## Could Use
- layouts/shortcodes/fancybox.html
```go

{{% fancybox path="/post/2019/img" file="k8.png" caption="golly" gallery="the-met" %}}

{{%/* fig class="full"
    src="http://farm5.staticflickr.com/4136/4829260124_57712e570a_o_d.jpg"
    title="One of my favorite touristy-type photos. I secretly waited for the
    good light while we were having fun and took this. Only regret: a stupid
    pole in the top-left corner of the frame I had to clumsily get rid of at
    post-processing."
    link="http://www.flickr.com/photos/alexnormand/4829260124/in/
            set-72157624547713078/" */%}}
```

## archive.org embeds
```html
<iframe src="https://archive.org/embed/ID" width="854" height="480" frameborder="0" webkitallowfullscreen="true" mozallowfullscreen="true" allowfullscreen></iframe>
```

---

## theme setup
- https://themes.gohugo.io/hugo-future-imperfect-slim/
  - `mkdir -p themes`
  - `cd themes`
  - `git submodule add https://github.com/pacollins/hugo-future-imperfect-slim`
  - or if cloned to another machine:
  - `git submodule update --init --recursive`


## initial setup
- https://gohugo.io/hosting-and-deployment/hosting-on-gitlab/
- https://about.gitlab.com/2016/04/07/gitlab-pages-setup/
- https://github.com/pacollins/hugo-future-imperfect-slim/wiki/Staticman-config
- added Git LFS (esp. for imagery / big files and if i future resize/recrop, etc.)
  - `git lfs track "*.jpg"`
- had to sort out http://localhost:1313/post/
- https://gohugo.io/content-management/shortcodes/#youtube
- archive.org video/book embeds
- `hugo` # build public
- `hugo new post/my-first-post.md`


## run and/or make `public/` subdir
- `brew install hugo`
- ensures fresh - removes prior run
- [gogo](gogo)
  - `CTL-C` at any point..


## comments ingesting
https://yasoob.me/posts/running_staticman_on_static_hugo_blog_with_nested_comments/

```bash
wget -qO- 'https://poohbot.com/wp-json/wp/v2/comments?per_page=100' |jq .

cd ~/poohbot; ./comments2json; line; files data/comments|lc; line; grep -h author_url *.json|sort|uniq -c|sort -n; line; grep -rh '"website": ' data/comments/|sort|uniq -c|sort -n; line; files data/comments/|lc

echo -n post/2019/09-techo-tuesday-make-a-free-website-static-site-generators-and-hugo.md |md5
```
