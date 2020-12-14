---
id: 112
title: Deleting files within a directory with a given extension
date: 2015-07-05T06:13:12+10:00
author: Stewart Polley
layout: post
guid: http://stewpolley.local/?p=112
permalink: /2015/07/05/deleting-files-within-a-directory-with-a-given-extension/
mfn-post-hide-content:
  - "0"
mfn-post-slider:
  - "0"
mfn-post-slider-layer:
  - "0"
mfn-post-hide-title:
  - "0"
mfn-post-remove-padding:
  - "0"
mfn-post-intro:
  - 'a:1:{s:9:"post-meta";s:1:"1";}'
mfn-post-hide-image:
  - "0"
mfn-post-love:
  - "1"
image: /wp-content/uploads/2017/12/opengraph-icon-200x200-146x146.png
categories:
  - Old
  - Python
---
I wanted to delete a large number of .SRT files from some TV shows I had. (Sub-title files)

Going through each folder + selecting the files individually is tedious.

I wrote this bit of code, with the help of this Stack Overflow question.

Save this to the directory you want to delete the files from.

`python remove_srt.py extension1 extension2`

For example, to remove all JPG, TXT and PNG files:

`python remove_srt.py jpg txt png`

Script is here:

<pre>import sys
import os

num_deleted = 0
num_skipped = 0

def scandirs(path, extensions):
    global num_deleted
    global num_skipped
    for root, dirs, files in os.walk(path):
        for currentFile in files:
            print("processing file: " + currentFile)
            if any(currentFile.lower().endswith(ext) for ext in extensions):
                os.remove(os.path.join(root, currentFile))
                print("{} deleted".format(os.path.join(root, currentFile)))
                num_deleted += 1
            else:
                num_skipped += 1


if __name__ == '__main__':
    extensions = []
    for arg in sys.argv[1:]:
        
        extensions.append(arg)
    path = os.getcwd()
    scandirs(path, extensions)
    print("{} files deleted".format(num_deleted))
    print("{} files skipped".format(num_skipped))
</pre>

* * *

### Dec 2017 update

Oh boy, how time flies and how we learn. I&#8217;ll get a post up in early 2018 with a better way of doing this.