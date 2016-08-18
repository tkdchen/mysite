---
layout: post
title: Ensure ABI compliance with fedabipkgdiff
date: 2016-08-03
category: blog
tags: Fedora libabigail
---

In the past days, I have been hacking on
[libabigail](https://sourceware.org/libabigail/) to write a tool named
fedabipkgdiff. That came from a cool idea of Dodji Seketeli who is the author
of libabigail and is built on top of the powerful libabigail framework. Just
imagine, if you are a Fedora developer or packager, when you want to ensure
the new version software is ABI-compatible with previous version or versions,
how would you do that? If there are already built builds in Koji, if you want
to see what ABI differences they have, then how to do that? fedabipkgdiff is
able to make your life easier.

fedabipkgdiff is designed to be as smart as possible to find necessary RPMs
from local or Koji, and run ABI check automatically on behalf of you. All you
need to do is just to specify

- from where to find RPMs, which defaults to Koji obviously
- what RPMs are needed, to select RPMs from latest build of a distribution
  such as Fedora 24, 25, or from a specific build with ``N-V-R`` such as
  ``httpd-2.4.23-4.fc24``
- how RPMs will be compared, that means you have a chance to let fedabipkgdiff
  do not compare with development package, you can also tell it to compare
  without loading suppression file

Okay, now I'll show you some examples to see what fedabipkgdiff can do.

First, I want to compare package httpd selected from latest build built for
Fedora 24 and 25. Run this command


<pre>
fedabipkgdiff --from 24 --to 25 httpd
</pre>

You will get

<pre>
Comparing the ABI of binaries between httpd-2.4.23-4.fc23.armv7hl.rpm and httpd-2.4.23-4.fc24.armv7hl.rpm:

================ changes of 'httpd'===============
  Functions changes summary: 0 Removed, 1 Changed (224 filtered out), 0 Added function
  Variables changes summary: 0 Removed, 0 Changed (10 filtered out), 0 Added variable

  1 function with some indirect sub-type change:

    [C]'function void ap_add_file_conf(apr_pool_t*, core_dir_config*, void*)' at core.c:572:1 has some indirect sub-type changes:
      parameter 2 of type 'core_dir_config*' has sub-type changes:
        in pointed to type 'typedef core_dir_config' at http_core.h:675:1:
          underlying type 'struct __anonymous_struct__' at http_core.h:518:1 changed:
            3 data member changes:
             'unsigned int __anonymous_struct__::d_is_fnmatch' offset changed from 192 to 200 (in bits)
             'unsigned int __anonymous_struct__::add_default_charset' offset changed from 192 to 200 (in bits)
             'unsigned int __anonymous_struct__::condition_ifelse' offset changed from 864 to 872 (in bits)


================ end of changes of 'httpd'===============

================ changes of 'mod_cache.so'===============
  Functions changes summary: 0 Removed, 1 Changed (17 filtered out), 0 Added function
  Variables changes summary: 0 Removed, 0 Changed, 0 Added variable

  1 function with some indirect sub-type change:

    [C]'function int ap_cache_control(request_rec*, cache_control_t*, const char*, const char*, apr_table_t*)' at cache_util.c:965:1 has some indirect sub-type changes:
      parameter 2 of type 'cache_control_t*' has sub-type changes:
        in pointed to type 'typedef cache_control_t' at cache_common.h:53:1:
          underlying type 'struct cache_control' at cache_common.h:30:1 changed:
            10 data member changes:
             'unsigned int cache_control::public' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::only_if_cached' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::s_maxage' offset changed from 0 to 16 (in bits)
             'unsigned int cache_control::private_header' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::must_revalidate' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::no_transform' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::private' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::proxy_revalidate' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::min_fresh' offset changed from 0 to 8 (in bits)
             'unsigned int cache_control::invalidated' offset changed from 0 to 16 (in bits)


================ end of changes of 'mod_cache.so'===============

================ changes of 'mod_dav_fs.so'===============
  Functions changes summary: 0 Removed, 1 Changed (6 filtered out), 0 Added function
  Variables changes summary: 0 Removed, 0 Changed (2 filtered out), 0 Added variable

  1 function with some indirect sub-type change:

    [C]'function dav_error* dav_fs_dir_file_name(const dav_resource*, const char**, const char**)' at repos.c:237:1 has some indirect sub-type changes:
      parameter 1 of type 'const dav_resource*' has sub-type changes:
        in pointed to type 'const dav_resource':
          in unqualified underlying type 'typedef dav_resource' at mod_dav.h:402:1:
            underlying type 'struct dav_resource' at mod_dav.h:368:1 changed:
...

================ end of changes of 'mod_dav_fs.so'===============

================ changes of 'mod_dav_lock.so'===============
  Functions changes summary: 0 Removed, 0 Changed (1 filtered out), 0 Added function
  Variables changes summary: 0 Removed, 1 Changed, 0 Added variable

  1 Changed variable:

    [C]'const dav_hooks_locks dav_hooks_locks_generic' was changed at locks.c:202:1:
      type of variable changed:
       in unqualified underlying type 'typedef dav_hooks_locks' at mod_dav.h:249:1:
         underlying type 'struct dav_hooks_locks' at mod_dav.h:1314:1 changed:
           1 data member changes (11 filtered):
            type of 'dav_error* (dav_lockdb*, const dav_resource*, int, const dav_lock*)* dav_hooks_locks::append_locks' changed:
              in pointed to type 'function type dav_error* (dav_lockdb*, const dav_resource*, int, const dav_lock*)':
...

================ end of changes of 'mod_dav_lock.so'===============

================ changes of 'mod_proxy.so'===============
  Functions changes summary: 0 Removed, 1 Changed (51 filtered out), 0 Added function
  Variables changes summary: 0 Removed, 0 Changed, 0 Added variable

  1 function with some indirect sub-type change:

    [C]'function int ap_proxy_acquire_connection(const char*, proxy_conn_rec**, proxy_worker*, server_rec*)' at proxy_util.c:2114:1 has some indirect sub-type changes:
      parameter 2 of type 'proxy_conn_rec**' has sub-type changes:
        in pointed to type 'proxy_conn_rec*':
          in pointed to type 'typedef proxy_conn_rec' at mod_proxy.h:274:1:
            underlying type 'struct __anonymous_struct__' at mod_proxy.h:253:1 changed:
              5 data member changes (2 filtered):
               type of 'proxy_worker* __anonymous_struct__::worker' changed:
                 in pointed to type 'typedef proxy_worker' at mod_proxy.h:109:1:
...

================ end of changes of 'mod_proxy.so'===============

================ changes of 'mod_proxy_ajp.so'===============
  Functions changes summary: 0 Removed, 1 Changed (7 filtered out), 0 Added function
  Variables changes summary: 0 Removed, 0 Changed, 0 Added variable

  1 function with some indirect sub-type change:

    [C]'function apr_status_t ajp_parse_header(request_rec*, proxy_dir_conf*, ajp_msg_t*)' at ajp_header.c:763:1 has some indirect sub-type changes:
      parameter 2 of type 'proxy_dir_conf*' has sub-type changes:
        in pointed to type 'typedef proxy_dir_conf' at mod_proxy.h:242:1:
          underlying type 'struct __anonymous_struct__' at mod_proxy.h:205:1 changed:
            1 data member change:
             type of 'proxy_alias* __anonymous_struct__::alias' changed:
               in pointed to type 'struct proxy_alias' at mod_proxy.h:126:1:
                 1 data member change:
...

================ end of changes of 'mod_proxy_ajp.so'===============
</pre>

The output from fedabipkgdiff has a regular format. For each section of
comparison between versions of RPMs, fedabipkgdiff displays ABI difference of
each binary one by one. As shown above, its format follows.

<pre>
Comparing the ABI of binaries between foo-N-V-R.A.rpm and httpd-N-V-R.A.rpm:

================ changes of 'binary name'===============

  { summar lines }

  { a list of changed functions with details }

================ end of changes of 'httpd'===============
</pre>

Second, I want to compare package httpd from two builds with specific ``N-V-R``
rather than the latest one. Run this command

<pre>
fedabipkgdiff httpd-2.4.23-3.fc23 httpd-2.4.23-3.fc24
</pre>

As a result, you will ABI differences in similar format like above. This
command compares all RPMs with all arches.

Furthermore, if it is just necessary to compare RPMs with arch ``x86_64``, I
can do in this way,

<pre>
fedabipkgdiff httpd-2.4.23-3.fc23.x86_64 httpd-2.4.23-3.fc24.x86_64
</pre>

fedabipkgdiff is trying to compare ABI of binaries as smart as possible for
Fedora developers and packagers. Besides what I have shown to you above, you
can do more with fedabipkgdiff by specifying various options. For more
details, please refer to [official
manual](https://sourceware.org/libabigail/manual/fedabipkgdiff.html), or ``man
fedabipkgdiff``.
