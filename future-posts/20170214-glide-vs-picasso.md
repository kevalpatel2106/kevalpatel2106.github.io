title: Glide vs. Picasso
link: https://kevalpatel2106.wordpress.com/2017/02/14/glide-vs-picasso/
author: kevalpatel2106
description: 
post_id: 399
created: 2017/02/14 07:41:34
created_gmt: 2017/02/14 02:11:34
comment_status: open
post_name: glide-vs-picasso
status: publish
post_type: post

# Glide vs. Picasso

Glide and Picasso are the most used image loading library in the world of android applications. Most of the android application developers have used any of these or both libraries in their career. Both libraries provide number of features, very fast and optimized. They are also well tested on many applications. On the surface, it looks like both are working on the same mechanism. But they both are very different in the way how they download images, caches images and loads them into memory. Today, we are going to look into the core differences between both the libraries and try to figure out which one is the best for application development. 

> We are comparing Glide v3.7.0 and Picasso v2.5.2 versions for this article. The latest available version might be different depending on when you are reading this article.

## Import to the project:

Picasso and Glide, both are on jcenter. You can simply import those into your project with below dependency.

### Picasso:

[gist]0456520517ce81e54b1888915bdea14a[/gist]

### Glide:

[gist]49e6aacd62a472cbf77ad81dac72efa6[/gist]

> _support-v4 _dependency is not needed for most of the projects as most of them uses support library by default.

## Size and Method Count:

While comparing the sizes of the .jar file of both the libraries, Glide is almost 3.5 times larger than Picasso in size.

![1rqe23b0xa6fpd6zopjx2aq](https://kevalpatel2106.files.wordpress.com/2017/02/1rqe23b0xa6fpd6zopjx2aq.png)

Picasso has method count of 849, while Glide has total 2678 method count. 2678 is quite a lot for 65535 methods limit of Android DEX file. You should enable ProGuard if you choose to use Glide.

![1se-djihhpw8iwf4gij66aa](https://kevalpatel2106.files.wordpress.com/2017/02/1se-djihhpw8iwf4gij66aa.png)

## Syntax:

Both libraries have almost same syntax if you want to simply load the image from the URL and display them into the image view. Both support the fade animations and center crop. You can also add placeholder image to display while loading the image or while the image loading fails.

### Glide:

[gist]b93fcbcd547f15de654bc1091b6abc30[/gist] 

### Picasso:

[gist]e9187f99a1b40092dcedd01bcc9a9a35[/gist] One thing in which Glide dominates is, it is designed to work with Activity and Fragment’s life cycle too. You can pass the activity or fragment context with _Glide.with()_ and it will brilliantly integrate with activity lifecycle callbacks such as _onPause()_ or _onResume()_. ![1*Luh0ueUaCghrAywvkMxE4Q.png](https://kevalpatel2106.files.wordpress.com/2017/02/1luh0ueuacghraywvkmxe4q.png)

## Disk caching:

Both the library supports caching the image in the disk. They download the images from the URL and store those images on the disk by caching. But there are some differences how they store the images in the cache.

Picasso downloads the image and stores the full-size image (in my case the image resolution was 1160*750) in the cache and whenever we ask for the same image, it will return the full-size image and resize them to fit into the ImageView in real time.

On the other hand, Glide works differently. Glide downloads the image from the given URL, resize it to the size of the image view and stores it to the disk cache. So if you are loading the same image in two different sized image views, Glide will store two different copies of the same image in the cache with different resolutions. That will increase the disk cache size, but it has some of its own benefits. We will see that in the next section.

When I tried to adjust ImageView to the different sizes, Picasso cached only single size of the image and that is full-size. While Glide caches separate files for each size of ImageView. On down point of this approach is, although an image has already been loaded once, if you need to load the image in another size ImageView, it needs to be downloaded once again before be resized to the right resolution and then be cached.

## Memory:

By default, Glide uses **RGB_555** configuration while Picasso loads images in **ARGB_8888** configuration to load the bitmap into memory. So for fair comparison, I made some changes in GlideModule to load images in ARGB_8888 format by creating a new class which extended from GlideModule like this:

[gist]de101a4dd7d2f3f1a4cb1e22d1d078ba[/gist]

Below is the graph of memory consumption of loading the image in Glide and Picasso:

![1*-TOjFF8NJ6W7bmHZycjDtw.png](https://kevalpatel2106.files.wordpress.com/2017/02/1-tojff8nj6w7bmhzycjdtw.png)

By looking into the graph, we can see that Glide is more memory efficient (about 8 MB) than Picasso (about 13 MB). This is pretty much understandable as we discussed in the earlier section, that Picasso loads the full-size image into the memory and relies on GPU to resize that image to fit into the size of the ImageView. While Glide loads an image that is already resized as per the ImageView, that requires significantly less memory than loading the full image like Picasso. This helps to prevent your app from throwing popular OutOfMemoryError.

## Time taken to load the Image:

When we try to load an image from the URL, both libraries checks their local caches and if the image is not present in the cache, they will download the image from the URL.