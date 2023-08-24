# Project 3 (Part A) - Data Wrangling

MISM 6205 <br>
Name: Soham Patki<br>
Professor: Dr. Tareq Nasralah<br>
Date : 12/2/22


```python
from bs4 import BeautifulSoup
import requests
import pandas as pd
import math
import warnings
warnings.filterwarnings('ignore')
```

# Helper functions:


```python
"""
The read_html(url) return the contents of an URL hmtl file as a string.
"""
def read_html(url):
    response = requests.get(url)
    content = response.content
    return content

#===========================================================================

"""
If a string contains comma, use "" to enclose the string to write it to a comma-separated values (CSV) 
file with no issue with the comma.
"""
def process_str_with_comma(string):
    if ',' in string:
        new_string = '"' + string.strip() + '"'
    else:
        new_string = string
    return new_string

```

### Test  read_html 


```python
def test_read_html():
    print (read_html('https://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=1&title_type=feature&year=2018,2020'))

```


```python
test_read_html()
```

    b'\n\n<!DOCTYPE html>\n<html\n    xmlns:og="http://ogp.me/ns#"\n    xmlns:fb="http://www.facebook.com/2008/fbml">\n    <head>\n         \n\n        <meta charset="utf-8">\n\n\n\n\n        <script type="text/javascript">var IMDbTimer={starttime: new Date().getTime(),pt:\'java\'};</script>\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "LoadTitle", {wb: 1});\n    }\n</script>\n  <script>(function(t){ (t.events = t.events || {})["csm_head_pre_title"] = new Date().getTime(); })(IMDbTimer);</script>\n        <title>Feature Film,\nReleased between 2018-01-01 and 2020-12-31\n(Sorted by Number of Votes Descending) - IMDb</title>\n  <script>(function(t){ (t.events = t.events || {})["csm_head_post_title"] = new Date().getTime(); })(IMDbTimer);</script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "LoadTitle", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "LoadTitle", {wb: 1});\n    }\n</script>\n\n        <link rel="canonical" href="https://www.imdb.com/search/title/?title_type=feature&year=2018-01-01,2020-12-31" />\n        <meta property="og:url" content="http://www.imdb.com/search/title/?title_type=feature&year=2018-01-01,2020-12-31" />\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "LoadIcons", {wb: 1});\n    }\n</script>\n  <script>(function(t){ (t.events = t.events || {})["csm_head_pre_icon"] = new Date().getTime(); })(IMDbTimer);</script>\n\n        <link rel=\'icon\' sizes=\'32x32\' href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/favicon_desktop_32x32._CB1582158068_.png\' />\n        <link rel=\'icon\' sizes=\'167x167\' href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/favicon_iPad_retina_167x167._CB1582158068_.png\' />\n        <link rel=\'icon\' sizes=\'180x180\' href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/favicon_iPhone_retina_180x180._CB1582158069_.png\' />\n\n        <link href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/apple-touch-icon-mobile._CB479963088_.png\' rel=\'apple-touch-icon\' />\n        <link href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/apple-touch-icon-mobile-76x76._CB479962152_.png\' rel=\'apple-touch-icon\' sizes=\'76x76\' />\n        <link href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/apple-touch-icon-mobile-120x120._CB479963088_.png\' rel=\'apple-touch-icon\' sizes=\'120x120\' />\n        <link href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/apple-touch-icon-web-152x152._CB479963088_.png\' rel=\'apple-touch-icon\' sizes=\'152x152\' />\n\n        <link href=\'https://m.media-amazon.com/images/G/01/imdb/images-ANDW73HA/android-mobile-196x196._CB479962153_.png\' rel=\'shortcut icon\' sizes=\'196x196\' />\n\n        <meta name="theme-color" content="#000000" />\n         \n        <link rel="search" type="application/opensearchdescription+xml" href="https://m.media-amazon.com/images/S/sash/MzfIBMq9GBucYqW.xml" title="IMDb" />\n  <script>(function(t){ (t.events = t.events || {})["csm_head_post_icon"] = new Date().getTime(); })(IMDbTimer);</script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "LoadIcons", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "LoadIcons", {wb: 1});\n    }\n</script>\n\n        <meta property="pageType" content="advsearch" />\n        <meta property="subpageType" content="title" />\n\n\n        <link rel=\'image_src\' href="https://m.media-amazon.com/images/G/01/imdb/images/social/imdb_logo._CB410901634_.png">\n        <meta property=\'og:image\' content="https://m.media-amazon.com/images/G/01/imdb/images/social/imdb_logo._CB410901634_.png" />\n\n    <meta property=\'fb:app_id\' content=\'115109575169727\' />\n\n      <meta property=\'og:title\' content="Feature Film,\nReleased between 2018-01-01 and 2020-12-31\n(Sorted by Number of Votes Descending) - IMDb" />\n    <meta property=\'og:site_name\' content=\'IMDb\' />\n    <meta name="title" content="Feature Film,\nReleased between 2018-01-01 and 2020-12-31\n(Sorted by Number of Votes Descending) - IMDb" />\n        <meta name="description" content="IMDb\'s advanced search allows you to run extremely powerful queries over all people and titles in the database. Find exactly what you\'re looking for!" />\n        <meta property="og:description" content="IMDb\'s advanced search allows you to run extremely powerful queries over all people and titles in the database. Find exactly what you\'re looking for!" />\n        <meta name="request_id" content="X4C9E80X19W8HRAFKK6D" />\n\n    <script>\n        (function (win) {\n            win.PLAID_LOAD_FONTS_FIRED = true;\n\n            if (typeof win.FontFace !== "undefined"\n                && typeof win.Promise !== "undefined") {\n                if (win.ue) {\n                    win.uet("bb", "LoadRoboto", { wb: 1 });\n                }\n                var allowableLoadTime = 1000;\n                var startTimeInt = +new Date();\n                var roboto = new FontFace(\'Roboto\',\n                    \'url(https://m.media-amazon.com/images/G/01/IMDb/cm9ib3Rv.woff2)\',\n                    { style:\'normal\', weight: 400 });\n                var robotoMedium = new FontFace(\'Roboto\',\n                    \'url(https://m.media-amazon.com/images/G/01/IMDb/cm9ib3RvTWVk.woff2)\',\n                    { style:\'normal\', weight: 500 });\n                var robotoBold = new FontFace(\'Roboto\',\n                    \'url(https://m.media-amazon.com/images/G/01/IMDb/cm9ib3RvQm9sZA.woff2)\',\n                    { style:\'normal\', weight: 600 });\n                var robotoLoaded = roboto.load();\n                var robotoMediumLoaded = robotoMedium.load();\n                var robotoBoldLoaded = robotoBold.load();\n\n                win.Promise.all([robotoLoaded, robotoMediumLoaded, robotoBoldLoaded]).then(function() {\n                    var loadTimeInt = +new Date();\n                    var robotoLoadedCount = 0;\n                    if ((loadTimeInt - startTimeInt) <= allowableLoadTime) {\n                        win.document.fonts.add(roboto);\n                        win.document.fonts.add(robotoMedium);\n                        win.document.fonts.add(robotoBold);\n                        robotoLoadedCount++;\n                    }\n                    if (win.ue) {\n                        win.ue.count("roboto-loaded", robotoLoadedCount);\n                        win.uet("be", "LoadRoboto", { wb: 1 });\n                        win.uex("ld", "LoadRoboto", { wb: 1 });\n                    }\n                }).catch(function() {\n                    if (win.ue) {\n                        win.ue.count("roboto-loaded", 0);\n                    }\n                });\n            } else {\n                if (win.ue) {\n                    win.ue.count("roboto-load-not-attempted", 1);\n                }\n            }\n        })(window);\n    </script>\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "LoadCSS", {wb: 1});\n    }\n</script>\n  <script>(function(t){ (t.events = t.events || {})["csm_head_pre_css"] = new Date().getTime(); })(IMDbTimer);</script>\n\n\n<link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/dYCn3sO3MTNtxc0.css" />\n<!-- h=ics-c52xl-15-1f-26cc77c9.us-east-1 -->\n<!--[if IE]><link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/pXHSPBTKPo0GIjW.css" /><![endif]-->\n\n            <link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/vEv7EhTS45PyD8e.css" />\n            <link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/i-Y65BmJrKWIfbK.css" />\n            <link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/HqcrRlviNTScutU.css" />\n            <link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/mrwUCCyp3V14GU0.css" />\n            <link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/sHfJ8wTsDnFAHcY.css" />\n        <noscript>\n            <link rel="stylesheet" type="text/css" href="https://m.media-amazon.com/images/S/sash/CCc6Ja$8QUPPKkY.css">\n        </noscript>\n  <script>(function(t){ (t.events = t.events || {})["csm_head_post_css"] = new Date().getTime(); })(IMDbTimer);</script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "LoadCSS", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "LoadCSS", {wb: 1});\n    }\n</script>\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "LoadJS", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "LoadHeaderJS", {wb: 1});\n    }\n</script>\n  <script>(function(t){ (t.events = t.events || {})["csm_head_pre_ads"] = new Date().getTime(); })(IMDbTimer);</script>\n        \n        <script  type="text/javascript">\n            // ensures js doesn\'t die if ads service fails.  \n            // Note that we need to define the js here, since ad js is being rendered inline after this.\n            (function(f) {\n                // Fallback javascript, when the ad Service call fails.  \n                \n                if((window.csm == null || window.generic == null || window.consoleLog == null)) {\n                    if (window.console && console.log) {\n                        console.log("one or more of window.csm, window.generic or window.consoleLog has been stubbed...");\n                    }\n                }\n                \n                window.csm = window.csm || { measure:f, record:f, duration:f, listen:f, metrics:{} };\n                window.generic = window.generic || { monitoring: { start_timing: f, stop_timing: f } };\n                window.consoleLog = window.consoleLog || f;\n            })(function() {});\n        </script>\n\n        <script type="text/javascript">window.useRatingTaskCompletion = true;</script>\n\n  <script>\n    if (\'csm\' in window) {\n      csm.measure(\'csm_head_delivery_finished\');\n    }\n  </script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "LoadHeaderJS", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "LoadHeaderJS", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "LoadJS", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "LoadJS", {wb: 1});\n    }\n</script>\n\n        \n    </head>\n    <body id="styleguide-v2" class="fixed">\n        \n<script>\n    if (typeof uet == \'function\') {\n      uet("bb");\n    }\n</script>\n  <script>\n    if (\'csm\' in window) {\n      csm.measure(\'csm_body_delivery_started\');\n    }\n  </script>\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("ns");\n    }\n</script>\n\n<style data-styled="true" data-styled-version="5.3.5">.hKcFfj{-webkit-flex-shrink:0;-ms-flex-negative:0;flex-shrink:0;display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;-webkit-box-pack:center;-webkit-justify-content:center;-ms-flex-pack:center;justify-content:center;-webkit-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none;margin-left:0.25rem;margin-right:auto;-webkit-order:1;-ms-flex-order:1;order:1;position:relative;}/*!sc*/\n@media screen and (min-width:1024px){.hKcFfj{margin-left:auto;margin-right:0.5rem;-webkit-order:0;-ms-flex-order:0;order:0;padding-left:0;}}/*!sc*/\n@media (hover:hover) and (pointer:fine){.hKcFfj:focus{outline:1px dashed currentColor;}.hKcFfj:focus:active{outline:0;}.hKcFfj:before,.hKcFfj:after{border-radius:10%;bottom:0;content:\'\';height:100%;left:0;margin:auto;opacity:0;position:absolute;right:0;top:0;-webkit-transition:opacity 0.2s cubic-bezier(1,1,1,1);transition:opacity 0.2s cubic-bezier(1,1,1,1);width:100%;background:rgb(0,0,0);background:var(--ipt-on-accent1-color,rgb(0,0,0));}.hKcFfj:hover:before{opacity:0.08;opacity:var(--ipt-base-hover-opacity,0.08);}.hKcFfj:active:after{opacity:0.16;opacity:var(--ipt-base-pressed-opacity,0.16);}}/*!sc*/\ndata-styled.g1[id="sc-bczRLJ"]{content:"hKcFfj,"}/*!sc*/\n.lbYfkb li:first-child > .searchResult{border-top:none;}/*!sc*/\n.lbYfkb .searchTypeahead__input{font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:1rem;font-size:var(--ipt-type-body-size,1rem);font-weight:400;font-weight:var(--ipt-type-body-weight,400);-webkit-letter-spacing:.03125em;-moz-letter-spacing:.03125em;-ms-letter-spacing:.03125em;letter-spacing:.03125em;-webkit-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-moz-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-ms-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);line-height:1.5rem;line-height:var(--ipt-type-body-lineHeight,1.5rem);text-transform:none;text-transform:var(--ipt-type-body-textTransform,none);color:rgb(255,255,255);color:var(--ipt-on-baseAlt-color,rgb(255,255,255));caret-color:rgb(255,255,255);caret-color:var(--ipt-on-baseAlt-color,rgb(255,255,255));}/*!sc*/\n.lbYfkb .searchTypeahead__input:placeholder{color:rgba(255,255,255,0.5);color:var(--ipt-on-baseAlt-textHint-color,rgba(255,255,255,0.5));}/*!sc*/\n@media screen and (min-width:600px){.lbYfkb .searchTypeahead__input{font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:.875rem;font-size:var(--ipt-type-bodySmall-size,.875rem);font-weight:400;font-weight:var(--ipt-type-bodySmall-weight,400);-webkit-letter-spacing:.01786em;-moz-letter-spacing:.01786em;-ms-letter-spacing:.01786em;letter-spacing:.01786em;-webkit-letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);-moz-letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);-ms-letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);line-height:1.25rem;line-height:var(--ipt-type-bodySmall-lineHeight,1.25rem);text-transform:none;text-transform:var(--ipt-type-bodySmall-textTransform,none);color:rgba(0,0,0,0.87);color:var(--ipt-on-base-textPrimary-color,rgba(0,0,0,0.87));caret-color:rgba(0,0,0,0.54);caret-color:var(--ipt-on-base-textSecondary-color,rgba(0,0,0,0.54));}}/*!sc*/\ndata-styled.g7[id="sc-gKXOVf"]{content:"lbYfkb,"}/*!sc*/\n.iosfhR{position:relative;}/*!sc*/\n.iosfhR.navbar__flyout__text-button-after-mobile,.iosfhR .navbar__flyout__text-button-after-mobile{display:none;}/*!sc*/\n.iosfhR .navbar__flyout__text-button-after-mobile > div{display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;}/*!sc*/\n.iosfhR .navbar__flyout--menu{top:100%;position:absolute;margin-top:0.25rem;}/*!sc*/\n.iosfhR.iosfhR.navbar__flyout--positionLeft .navbar__flyout--menu{left:0;right:auto;}/*!sc*/\n@media screen and (min-width:600px){.iosfhR.navbar__flyout--breakpoint-m .navbar__flyout__icon-on-mobile{display:none;}.iosfhR.navbar__flyout--breakpoint-m .navbar__flyout__text-button-after-mobile{display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;}}/*!sc*/\n@media screen and (min-width:1024px){.iosfhR.navbar__flyout--breakpoint-l .navbar__flyout__icon-on-mobile{display:none;}.iosfhR.navbar__flyout--breakpoint-l .navbar__flyout__text-button-after-mobile{display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;}}/*!sc*/\n@media screen and (min-width:1280px){.iosfhR.navbar__flyout--breakpoint-xl .navbar__flyout__icon-on-mobile{display:none;}.iosfhR.navbar__flyout--breakpoint-xl .navbar__flyout__text-button-after-mobile{display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;}}/*!sc*/\n.iosfhR .navbar__flyout__button-pointer{-webkit-transition:-webkit-transform 0.2s;-webkit-transition:transform 0.2s;transition:transform 0.2s;-webkit-transform:rotateX(0);-ms-transform:rotateX(0);transform:rotateX(0);}/*!sc*/\n.iosfhR.navbar__flyout--isVisible .navbar__flyout__button-pointer{-webkit-transform:rotateX(180deg);-ms-transform:rotateX(180deg);transform:rotateX(180deg);}/*!sc*/\ndata-styled.g8[id="sc-iBkjds"]{content:"iosfhR,"}/*!sc*/\n.cumdHr .searchCatSelector__opener{border-radius:2px 0 0 2px;padding:0 0 0 0.5rem;min-height:32px;height:20px;border-right:1px solid rgba(0,0,0,0.3);}/*!sc*/\n.cumdHr .searchCatSelector .searchCatSelector__item{-webkit-transition:color 0.15s;transition:color 0.15s;cursor:poiner;}/*!sc*/\n.cumdHr .searchCatSelector .searchCatSelector__item .searchCatSelector__itemIcon{margin-right:0.75rem;opacity:0.5;-webkit-transition:opacity 0.2s;transition:opacity 0.2s;}/*!sc*/\n.cumdHr .searchCatSelector .searchCatSelector__item:hover .searchCatSelector__itemIcon,.cumdHr .searchCatSelector .searchCatSelector__item:focus .searchCatSelector__itemIcon,.cumdHr .searchCatSelector .searchCatSelector__item.searchCatSelector__itemSelected .searchCatSelector__itemIcon{opacity:1;}/*!sc*/\n.cumdHr .searchCatSelector .searchCatSelector__item.searchCatSelector__itemSelected{color:rgb(245,197,24);color:var(--ipt-on-base-accent1-color,rgb(245,197,24));}/*!sc*/\ndata-styled.g9[id="sc-ftvSup"]{content:"cumdHr,"}/*!sc*/\n.exojnx{background:rgb(31,31,31);background:var(--ipt-baseAlt-shade1-bg,rgb(31,31,31));display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-box-flex:1;-webkit-flex-grow:1;-ms-flex-positive:1;flex-grow:1;margin:0;padding:0;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;}/*!sc*/\n@media screen and (min-width:600px){.exojnx.searchform--focused .searchform__submit{background:transparent;opacity:1;}}/*!sc*/\n@media screen and (min-width:600px){.exojnx{-webkit-transition:border 0.2s,background-color 0.2s,box-shadow 0.2s;transition:border 0.2s,background-color 0.2s,box-shadow 0.2s;background:rgb(255,255,255);background:var(--ipt-base-bg,rgb(255,255,255));border-radius:.25rem;border-radius:var(--ipt-cornerRadius,.25rem);}.exojnx.searchform--focused{background:rgb(255,255,255);background:var(--ipt-base-bg,rgb(255,255,255));outline:none;border-color:none;box-shadow:inset 0 0 0 2px rgb(245,197,24);box-shadow:inset 0 0 0 2px var(--ipt-accent1-color,rgb(245,197,24));}}/*!sc*/\n.exojnx .searchform__inputContainer{width:100%;padding-right:3.5rem;}/*!sc*/\n.exojnx .searchform__submit{background:rgb(250,250,250);background:var(--ipt-base-shade1-bg,rgb(250,250,250));color:rgba(0,0,0,0.54);color:var(--ipt-on-base-textSecondary-color,rgba(0,0,0,0.54));border-radius:.25rem;border-radius:var(--ipt-cornerRadius,.25rem);position:absolute;right:0.35rem;min-width:2rem;cursor:pointer;top:0.35rem;-webkit-transition:all 0.2s;transition:all 0.2s;}/*!sc*/\n@media screen and (min-width:600px){.exojnx .searchform__submit{background:rgb(255,255,255);background:var(--ipt-base-bg,rgb(255,255,255));}}/*!sc*/\n.exojnx .imdb-header-search__input{background:transparent;-webkit-box-flex:1;-webkit-flex-grow:1;-ms-flex-positive:1;flex-grow:1;outline:none;padding:1rem 1rem 1rem 0.75rem;width:100%;}/*!sc*/\n@media screen and (min-width:600px){.exojnx .imdb-header-search__input{padding:0.375em 0 0.375rem 0.5rem;}}/*!sc*/\n.exojnx .imdb-header-search__input::-ms-clear{display:none;}/*!sc*/\ndata-styled.g10[id="sc-papXJ"]{content:"exojnx,"}/*!sc*/\n.iJNbPT{-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;left:0;margin:0;min-height:3.5rem;opacity:0;-webkit-transform:translateY(-10px);-ms-transform:translateY(-10px);transform:translateY(-10px);-webkit-transition:none;transition:none;-webkit-order:3;-ms-flex-order:3;order:3;pointer-events:none;position:absolute;top:0;visibility:hidden;width:100%;z-index:1;}/*!sc*/\n.iJNbPT .imdb-header-search__state-closer{-webkit-transform:scale(0.5);-ms-transform:scale(0.5);transform:scale(0.5);-webkit-transition:-webkit-transform 0.2s 0.1s;-webkit-transition:transform 0.2s 0.1s;transition:transform 0.2s 0.1s;display:inline;margin:0.25rem;position:absolute;right:0;top:0;}/*!sc*/\n.iJNbPT .imdb-header-search__state,.iJNbPT .imdb-header-search__input,.iJNbPT .nav-search__search-submit{-moz-appearance:none;-webkit-appearance:none;-webkit-appearance:none;-moz-appearance:none;appearance:none;border:none;}/*!sc*/\n@media screen and (min-width:600px){.iJNbPT{-webkit-box-flex:1;-webkit-flex-grow:1;-ms-flex-positive:1;flex-grow:1;margin:0 0.5rem;padding:0;min-height:2.25rem;-webkit-order:3;-ms-flex-order:3;order:3;opacity:1;visibility:visible;pointer-events:auto;position:relative;-webkit-transform:translateY(0);-ms-transform:translateY(0);transform:translateY(0);}.iJNbPT .imdb-header-search__state,.iJNbPT .nav-search__search-submit{padding:0;}.iJNbPT .nav-search__search-submit:focus{outline:var(--ipt-focus-outline-on-base);outline-offset:1px;}.iJNbPT .imdb-header-search__state-closer{display:none;}}/*!sc*/\ndata-styled.g11[id="sc-jqUVSM"]{content:"iJNbPT,"}/*!sc*/\n.cxVnln{-webkit-transition:all 0.3s;transition:all 0.3s;opacity:1;-webkit-transform:scale(1);-ms-transform:scale(1);transform:scale(1);-webkit-order:3;-ms-flex-order:3;order:3;}/*!sc*/\n@media screen and (min-width:600px){.cxVnln{-webkit-order:3;-ms-flex-order:3;order:3;}.cxVnln.imdb-header-search__state-opener{display:none;}}/*!sc*/\ndata-styled.g12[id="sc-kDDrLX"]{content:"cxVnln,"}/*!sc*/\n.iKpzZH:checked ~ .nav-search__search-container{background-color:rgb(0,0,0);background-color:var(--ipt-baseAlt-color,rgb(0,0,0));opacity:1;-webkit-transform:scale(1);-ms-transform:scale(1);transform:scale(1);-webkit-transition:opacity 0.2s,-webkit-transform 0.2s;-webkit-transition:opacity 0.2s,transform 0.2s;transition:opacity 0.2s,transform 0.2s;visibility:visible;pointer-events:auto;z-index:100;}/*!sc*/\n.iKpzZH:checked ~ .nav-search__search-container .nav-search__search-select,.iKpzZH:checked ~ .nav-search__search-container .nav-search__search-submit{display:none;}/*!sc*/\n.iKpzZH:checked ~ .nav-search__search-container .imdb-header-search__state-closer{-webkit-transform:scale(1);-ms-transform:scale(1);transform:scale(1);}/*!sc*/\n.iKpzZH:checked ~ .nav-search__search-container ~ .sc-kDDrLX{-webkit-transform:scale(0.5);-ms-transform:scale(0.5);transform:scale(0.5);-webkit-transition:all 0.3s 0.3s;transition:all 0.3s 0.3s;opacity:0;}/*!sc*/\ndata-styled.g13[id="sc-iqcoie"]{content:"iKpzZH,"}/*!sc*/\nbody.drawer-bodyLocked{overflow:hidden;position:relative;}/*!sc*/\ndata-styled.g14[id="sc-global-kcltSs1"]{content:"sc-global-kcltSs1,"}/*!sc*/\n.PMSTF.drawer{bottom:0;display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;left:0;overflow:hidden;-webkit-perspective:70vh;-moz-perspective:70vh;-ms-perspective:70vh;perspective:70vh;pointer-events:none;position:fixed;right:0;top:0;visibility:hidden;z-index:100;}/*!sc*/\n.PMSTF .drawer__panel{background:rgb(31,31,31);background:var(--ipt-baseAlt-shade1-bg,rgb(31,31,31));box-shadow:none;box-sizing:border-box;height:100%;overflow-x:hidden;overflow-y:auto;position:relative;-webkit-transform:translateX(calc(-100% - 36px));-ms-transform:translateX(calc(-100% - 36px));transform:translateX(calc(-100% - 36px));-webkit-transform-origin:right center;-ms-transform-origin:right center;transform-origin:right center;-webkit-transition:all 0.3s,box-shadow 0s;transition:all 0.3s,box-shadow 0s;width:280px;z-index:2;-webkit-overflow-scroll:touch;}/*!sc*/\n.PMSTF .drawer__backdrop{background:rgb(31,31,31);background:var(--ipt-baseAlt-shade1-bg,rgb(31,31,31));box-sizing:border-box;display:block;height:100%;left:0;opacity:0;position:absolute;top:0;-webkit-transition:opacity 0.3s;transition:opacity 0.3s;visibility:hidden;width:100%;will-change:opacity;z-index:1;}/*!sc*/\n.PMSTF .drawer__panelHeader{background:var(--ipt-baseAlt-stripes-bg);-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;box-sizing:border-box;display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-box-pack:end;-webkit-justify-content:flex-end;-ms-flex-pack:end;justify-content:flex-end;min-height:3.5rem;margin-bottom:0.5rem;padding:0.25rem;}/*!sc*/\n.PMSTF .drawer__panelHeaderClose{-webkit-touch-callout:none;-webkit-tap-highlight-color:transparent;}/*!sc*/\n.drawer__state:checked ~ .sc-crXcEl.drawer{pointer-events:auto;visibility:visible;}/*!sc*/\n.drawer__state:checked ~ .sc-crXcEl.drawer > .drawer__panel{-webkit-transform:translateX(0);-ms-transform:translateX(0);transform:translateX(0);box-shadow:0px 11px 15px -7px rgba(var(--ipt-baseAlt-rgb),0.2), 0px 24px 38px 3px rgba(var(--ipt-baseAlt-rgb),0.14), 0px 9px 46px 8px rgba(var(--ipt-baseAlt-rgb),0.12);}/*!sc*/\n.drawer__state:checked ~ .sc-crXcEl.drawer > .drawer__backdrop{opacity:0.5;visibility:visible;}/*!sc*/\n.PMSTF .drawer-logo{display:none;}/*!sc*/\n@media screen and (max-width:1024px){.PMSTF .drawer__panelHeader .drawer__panelHeaderClose{background:none;}}/*!sc*/\n@media screen and (min-width:1024px){.PMSTF .drawer__panel{width:100%;-webkit-transform:translateY(calc(-100%));-ms-transform:translateY(calc(-100%));transform:translateY(calc(-100%));padding:2rem 0;}.PMSTF .drawer__panelHeader{background:none;max-width:1024px;margin:auto;-webkit-box-pack:justify;-webkit-justify-content:space-between;-ms-flex-pack:justify;justify-content:space-between;padding:0 1rem;}.PMSTF .drawer__panelHeader .drawer__panelHeaderClose{background:rgb(245,197,24);background:var(--ipt-accent1-bg,rgb(245,197,24));color:rgb(0,0,0);color:var(--ipt-on-accent1-color,rgb(0,0,0));}.PMSTF .drawer__panelHeader .drawer__panelHeaderClose:hover{color:rgb(0,0,0);color:var(--ipt-on-accent1-color,rgb(0,0,0));}.PMSTF .drawer__panelContent{max-width:1024px;margin:auto;}.PMSTF .drawer-logo{display:inline;}}/*!sc*/\ndata-styled.g15[id="sc-crXcEl"]{content:"PMSTF,"}/*!sc*/\n.nMWbL .ipc-icon{opacity:0.5;-webkit-transition:opacity 0.2s;transition:opacity 0.2s;}/*!sc*/\n.nMWbL:hover .ipc-icon{opacity:1;}/*!sc*/\n@media screen and (max-width:479px){.nMWbL.nav-link--hideXS{display:none;}}/*!sc*/\n@media screen and (min-width:480px) and (max-width:599px){.nMWbL.nav-link--hideS{display:none;}}/*!sc*/\n@media screen and (min-width:600px) and (max-width:1023px){.nMWbL.nav-link--hideM{display:none;}}/*!sc*/\n@media screen and (min-width:1024px) and (max-width:1279px){.nMWbL.nav-link--hideL{display:none;}}/*!sc*/\n@media screen and (min-width:1280px){.nMWbL.nav-link--hideXL{display:none;}}/*!sc*/\ndata-styled.g16[id="sc-evZas"]{content:"nMWbL,"}/*!sc*/\n.cqozJK .navlinkcat__item{font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:1rem;font-size:var(--ipt-type-body-size,1rem);font-weight:400;font-weight:var(--ipt-type-body-weight,400);-webkit-letter-spacing:.03125em;-moz-letter-spacing:.03125em;-ms-letter-spacing:.03125em;letter-spacing:.03125em;-webkit-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-moz-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-ms-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);line-height:1.5rem;line-height:var(--ipt-type-body-lineHeight,1.5rem);text-transform:none;text-transform:var(--ipt-type-body-textTransform,none);cursor:pointer;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;border-top:1px solid transparent;display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-box-pack:justify;-webkit-justify-content:space-between;-ms-flex-pack:justify;justify-content:space-between;margin:0;padding:.5rem 1rem;-webkit-transition:color 0.1s ease-in,border-color 0.1s ease-in, opacity 0.12s ease-in;transition:color 0.1s ease-in,border-color 0.1s ease-in, opacity 0.12s ease-in;-webkit-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none;}/*!sc*/\n.cqozJK .navlinkcat__item:hover{background:rgba( rgb(255,255,255), 0.08 );background:rgba( var(--ipt-on-baseAlt-color,rgb(255,255,255)), var(--ipt-baseAlt-hover-opacity,0.08) );}/*!sc*/\n.cqozJK .navlinkcat__item:focus{outline:var(--ipt-focus-outline-on-baseAlt);outline-offset:1px;}/*!sc*/\n.cqozJK .navlinkcat__itemIcon{-webkit-align-self:flex-start;-ms-flex-item-align:start;align-self:flex-start;padding-right:0.75rem;}/*!sc*/\n.cqozJK .navlinkcat__itemTitle{-webkit-box-flex:1;-webkit-flex-grow:1;-ms-flex-positive:1;flex-grow:1;overflow:initial;overflow-wrap:break-word;padding-right:0.75rem;text-overflow:unset;white-space:break-spaces;}/*!sc*/\n@media screen and (min-width:1024px){.cqozJK .navlinkcat__itemTitle{font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:1.5rem;font-size:var(--ipt-type-headline5-size,1.5rem);font-weight:600;font-weight:var(--ipt-type-headline5-weight,600);-moz-osx-font-smoothing:grayscale;-webkit-font-smoothing:antialiased;-webkit-letter-spacing:normal;-moz-letter-spacing:normal;-ms-letter-spacing:normal;letter-spacing:normal;-webkit-letter-spacing:var(--ipt-type-headline5-letterSpacing,normal);-moz-letter-spacing:var(--ipt-type-headline5-letterSpacing,normal);-ms-letter-spacing:var(--ipt-type-headline5-letterSpacing,normal);letter-spacing:var(--ipt-type-headline5-letterSpacing,normal);line-height:2rem;line-height:var(--ipt-type-headline5-lineHeight,2rem);text-transform:none;text-transform:var(--ipt-type-headline5-textTransform,none);}}/*!sc*/\n.cqozJK .navlinkcat__itemChevron{-webkit-transform:rotate(90deg);-ms-transform:rotate(90deg);transform:rotate(90deg);}/*!sc*/\n.cqozJK .navlinkcat__itemIcon,.cqozJK .navlinkcat__itemChevron{opacity:0.5;-webkit-transition:all 0.2s;transition:all 0.2s;}/*!sc*/\n.cqozJK .navlinkcat__item:focus .navlinkcat__itemChevron,.cqozJK .navlinkcat__item:hover .navlinkcat__itemChevron,.cqozJK .navlinkcat__item:focus .navlinkcat__itemIcon,.cqozJK .navlinkcat__item:hover .navlinkcat__itemIcon{opacity:1;}/*!sc*/\n.cqozJK .navlinkcat__listContainer{height:0;visibility:hidden;overflow:hidden;border-bottom:1px solid transparent;-webkit-transition:border-color 0.1s ease-in,height 0.2s;transition:border-color 0.1s ease-in,height 0.2s;}/*!sc*/\n.cqozJK .navlinkcat__state:checked ~ .navlinkcat__targetWrapper .navlinkcat__item{border-color:rgba(255,255,255,0.2);border-color:var(--ipt-baseAlt-border-color,rgba(255,255,255,0.2));}/*!sc*/\n.cqozJK .navlinkcat__state:checked ~ .navlinkcat__targetWrapper .navlinkcat__item .navlinkcat__itemIcon,.cqozJK .navlinkcat__state:checked ~ .navlinkcat__targetWrapper .navlinkcat__item .navlinkcat__itemTitle{color:rgb(245,197,24);color:var(--ipt-on-baseAlt-accent1-color,rgb(245,197,24));}/*!sc*/\n.cqozJK .navlinkcat__state:checked ~ .navlinkcat__targetWrapper .navlinkcat__listContainer{border-color:rgba(255,255,255,0.2);border-color:var(--ipt-baseAlt-border-color,rgba(255,255,255,0.2));height:auto;visibility:inherit;}/*!sc*/\n.cqozJK .navlinkcat__state:checked ~ span .navlinkcat__item .navlinkcat__itemIcon{opacity:1;}/*!sc*/\n.cqozJK .navlinkcat__state:checked ~ span .navlinkcat__item .navlinkcat__itemChevron{-webkit-transform:rotate(-90deg);-ms-transform:rotate(-90deg);transform:rotate(-90deg);}/*!sc*/\n.cqozJK .navlinkcat__state:checked ~ span .navlinkcat__listContainer{display:block;}/*!sc*/\n.cqozJK:nth-of-type(1) .navlinkcat__item{border-top:none;}/*!sc*/\n@media screen and (min-width:1024px){.cqozJK.noMarginItem .navlinkcat__item{margin-top:0;}}/*!sc*/\n.cqozJK .ipc-list__item{height:auto;padding-top:.5rem;padding-bottom:.5rem;}/*!sc*/\n.cqozJK .ipc-list__item .ipc-list-item__text{white-space:initial;overflow-wrap:break-word;line-height:normal;}/*!sc*/\n@media screen and (min-width:1024px){.cqozJK{-webkit-flex-basis:33%;-ms-flex-preferred-size:33%;flex-basis:33%;max-width:33%;}.cqozJK .navlinkcat__item{border:none;margin-top:1.5rem;padding-top:.5rem;padding-bottom:.5rem;pointer-events:none;}.cqozJK .navlinkcat__state:checked ~ .navlinkcat__targetWrapper .navlinkcat__item .navlinkcat__itemTitle{color:inherit;}.cqozJK .navlinkcat__listContainer{visibility:inherit;height:auto !important;border:0;}.cqozJK .navlinkcat__itemChevron{display:none;}.cqozJK .navlinkcat__itemIcon{color:var(--ipt-on-baseAlt-accent1-color);opacity:1;}.cqozJK .ipc-list--baseAlt .ipc-list__item:hover{background:none;-webkit-text-decoration:underline;text-decoration:underline;}}/*!sc*/\n@media screen and (max-width:479px){.cqozJK.navlinkcat--hideXS{display:none;}}/*!sc*/\n@media screen and (min-width:480px) and (max-width:599px){.cqozJK.navlinkcat--hideS{display:none;}}/*!sc*/\n@media screen and (min-width:600px) and (max-width:1023px){.cqozJK.navlinkcat--hideM{display:none;}}/*!sc*/\n@media screen and (min-width:1024px) and (max-width:1279px){.cqozJK.navlinkcat--hideL{display:none;}}/*!sc*/\n@media screen and (min-width:1280px){.cqozJK.navlinkcat--hideXL{display:none;}}/*!sc*/\ndata-styled.g17[id="sc-breuTD"]{content:"cqozJK,"}/*!sc*/\n@media screen and (min-width:1024px){.gSOLIo{-webkit-flex-basis:33%;-ms-flex-preferred-size:33%;flex-basis:33%;max-width:33%;}}/*!sc*/\ndata-styled.g18[id="sc-ksZaOG"]{content:"gSOLIo,"}/*!sc*/\n@media screen and (min-width:1024px){.OzViy{-webkit-flex-basis:33%;-ms-flex-preferred-size:33%;flex-basis:33%;max-width:100%;}}/*!sc*/\ndata-styled.g19[id="sc-hAZoDl"]{content:"OzViy,"}/*!sc*/\n@media screen and (min-width:1024px){.SyRwP{max-width:100%;}}/*!sc*/\ndata-styled.g20[id="sc-fnykZs"]{content:"SyRwP,"}/*!sc*/\n.iYlngk .navlcl__divider{list-style:none;margin:0.5rem 0;opacity:0.2;}/*!sc*/\n.iYlngk .navlcl__proLink{margin-bottom:3rem;margin-top:1.5rem;padding:1rem;height:auto;}/*!sc*/\n.iYlngk .navlcl__proLink:hover,.iYlngk .navlcl__tvLink:hover{background:rgba( rgb(255,255,255), 0.08 );background:rgba( var(--ipt-on-baseAlt-color,rgb(255,255,255)), var(--ipt-baseAlt-hover-opacity,0.08) );}/*!sc*/\n@media screen and (min-width:1024px){.iYlngk{display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-flex-wrap:wrap;-ms-flex-wrap:wrap;flex-wrap:wrap;}.iYlngk .navlcl__tvLink{display:none;}.iYlngk .navlcl__proLink{-webkit-align-self:flex-start;-ms-flex-item-align:start;align-self:flex-start;display:none;}.iYlngk:focus{outline:var(--ipt-focus-outline-on-baseAlt);outline-offset:1px;}}/*!sc*/\ndata-styled.g21[id="sc-fEOsli"]{content:"iYlngk,"}/*!sc*/\n.bpTVou{font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:.875rem;font-size:var(--ipt-type-bodySmall-size,.875rem);font-weight:400;font-weight:var(--ipt-type-bodySmall-weight,400);-webkit-letter-spacing:.01786em;-moz-letter-spacing:.01786em;-ms-letter-spacing:.01786em;letter-spacing:.01786em;-webkit-letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);-moz-letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);-ms-letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);letter-spacing:var(--ipt-type-bodySmall-letterSpacing,.01786em);line-height:1.25rem;line-height:var(--ipt-type-bodySmall-lineHeight,1.25rem);text-transform:none;text-transform:var(--ipt-type-bodySmall-textTransform,none);display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-flex-direction:column;-ms-flex-direction:column;flex-direction:column;-webkit-box-pack:center;-webkit-justify-content:center;-ms-flex-pack:center;justify-content:center;}/*!sc*/\ndata-styled.g22[id="sc-bjUoiL"]{content:"bpTVou,"}/*!sc*/\n.cUVKHb{margin-top:.25rem;}/*!sc*/\ndata-styled.g23[id="sc-idiyUo"]{content:"cUVKHb,"}/*!sc*/\n.eSXHLQ{-webkit-flex-shrink:0;-ms-flex-negative:0;flex-shrink:0;-webkit-order:0;-ms-flex-order:0;order:0;}/*!sc*/\n@media screen and (min-width:1024px){.eSXHLQ{-webkit-order:1;-ms-flex-order:1;order:1;}}/*!sc*/\ndata-styled.g24[id="sc-dIouRR"]{content:"eSXHLQ,"}/*!sc*/\n.eIxWAw{-webkit-order:6;-ms-flex-order:6;order:6;}/*!sc*/\n@media screen and (min-width:600px){.eIxWAw .navbar__user-menu__username-divider,.eIxWAw .navbar__user-menu__username{display:none;}}/*!sc*/\n@media screen and (min-width:1024px){.eIxWAw{-webkit-order:7;-ms-flex-order:7;order:7;}}/*!sc*/\n.eIxWAw .navbar__user-menu-toggle__button{padding-right:0.25rem;}/*!sc*/\n.eIxWAw .navbar__user-name{max-width:160px;text-overflow:ellipsis;white-space:nowrap;overflow:hidden;}/*!sc*/\ndata-styled.g31[id="sc-cxabCf"]{content:"eIxWAw,"}/*!sc*/\nnav__userMenu .ipc-list__item.navbar__user-menu__username{color:rgb(245,197,24);color:var(--ipt-on-baseAlt-accent1-color,rgb(245,197,24));opacity:1;pointer-events:none;}/*!sc*/\ndata-styled.g32[id="sc-global-gbGWiL1"]{content:"sc-global-gbGWiL1,"}/*!sc*/\n.lkeUvO{-webkit-order:5;-ms-flex-order:5;order:5;display:none;}/*!sc*/\n@media screen and (min-width:1024px){.lkeUvO{-webkit-order:6;-ms-flex-order:6;order:6;display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;}}/*!sc*/\n.lkeUvO .watchlistButtonCount{background:rgb(245,197,24);background:var(--ipt-on-base-accent1-color,rgb(245,197,24));color:rgb(0,0,0);color:var(--ipt-on-accent1-color,rgb(0,0,0));font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:.75rem;font-size:var(--ipt-type-copyright-size,.75rem);font-weight:400;font-weight:var(--ipt-type-copyright-weight,400);-webkit-letter-spacing:.03333em;-moz-letter-spacing:.03333em;-ms-letter-spacing:.03333em;letter-spacing:.03333em;-webkit-letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);-moz-letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);-ms-letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);line-height:1rem;line-height:var(--ipt-type-copyright-lineHeight,1rem);text-transform:none;text-transform:var(--ipt-type-copyright-textTransform,none);margin-left:0.5rem;padding:0 0.4rem;border-radius:10px;text-align:center;}/*!sc*/\n.lkeUvO .ipc-button__text{display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;}/*!sc*/\ndata-styled.g33[id="sc-llJcti"]{content:"lkeUvO,"}/*!sc*/\n.ecLOfK{-webkit-order:4;-ms-flex-order:4;order:4;}/*!sc*/\n@media screen and (min-width:1024px){.ecLOfK{-webkit-order:4;-ms-flex-order:4;order:4;}}/*!sc*/\n.ecLOfK .navbar__imdbpro-menu-toggle__name{display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;}/*!sc*/\n.ecLOfK .navbar__imdbpro-content .navbar__flyout--menu{background-image:url(//m.media-amazon.com/images/G/01/imdb/images/navbar/imdbpro_navbar_menu_bg-3083451252._V_.png);background-size:cover;padding-left:17px;padding-bottom:25px;padding-top:25px;color:white;font-weight:bold;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad{background-repeat:no-repeat;color:white;cursor:pointer;width:573px;overflow:hidden;border-radius:8px;left:initial;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__content,.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__image{display:inline-block;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__title{color:white;line-height:1.3em;margin-bottom:10px;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__line{cursor:inherit;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__link{display:inline-block;-webkit-text-decoration:none;text-decoration:none;vertical-align:middle;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__content,.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__image{vertical-align:top;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__content{font-family:\'Arial\';margin-left:15px;width:400px;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__content .imdb-pro-ad__line{display:list-item;font-size:12px;list-style-position:inside;list-style-type:disc;margin:0px;padding:0.1rem 0;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-ad__content .imdb-pro-ad__title{font-size:15px;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-new__button{margin-top:15px;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-new__button text{fill:#111111;font-size:13px;font-weight:normal;}/*!sc*/\n.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-new__button svg:hover rect,.ecLOfK .navbar__imdbpro-imdb-pro-ad .imdb-pro-new__button text:hover rect{fill:#f7dd95;}/*!sc*/\n.ecLOfK .navbar__imdbpro-content .sub_nav{background-color:#f2f2f2;border-bottom-left-radius:10px;border-bottom-right-radius:10px;box-shadow:0 2px 5px rgba(0,0,0,0.6);color:#999;height:325px;}/*!sc*/\n.ecLOfK .navbar__imdbpro-content .sub_nav h5{color:#a58500;margin:20px 0 10px;position:relative;}/*!sc*/\ndata-styled.g34[id="sc-iIPllB"]{content:"ecLOfK,"}/*!sc*/\n.iwEoLL{display:none;}/*!sc*/\ndata-styled.g35[id="sc-gicCDI"]{content:"iwEoLL,"}/*!sc*/\n.crObts.crObts.selected{color:rgb(255,255,255);color:var(--ipt-on-baseAlt-color,rgb(255,255,255));font-weight:bold;pointer-events:none;}/*!sc*/\n.crObts.crObts.disabled{color:rgba(255,255,255,0.5);color:var(--ipt-on-baseAlt-textDisabled-color,rgba(255,255,255,0.5));}/*!sc*/\n.crObts.crObts span{opacity:1;}/*!sc*/\n.crObts .language-menu-item-span{padding-right:0.5rem;}/*!sc*/\n.crObts .language-menu-item-icon{height:1.5rem;width:1.5rem;}/*!sc*/\n.crObts .selected-language-icon{color:rgb(245,197,24);color:var(--ipt-accent1-color,rgb(245,197,24));}/*!sc*/\ndata-styled.g36[id="sc-ezWOiH"]{content:"crObts,"}/*!sc*/\n.cWEknO{-webkit-order:7;-ms-flex-order:7;order:7;}/*!sc*/\n@media screen and (min-width:600px){.cWEknO{-webkit-order:7;-ms-flex-order:7;order:7;}}/*!sc*/\n.cWEknO .navbar__flyout__text-button-after-mobile{display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;padding-right:0.25rem;}/*!sc*/\n.cWEknO .disabled{pointer-events:none;}/*!sc*/\ndata-styled.g39[id="sc-ikZpkk"]{content:"cWEknO,"}/*!sc*/\n.bIXmnE.bIXmnE{font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:.75rem;font-size:var(--ipt-type-overline-size,.75rem);font-weight:600;font-weight:var(--ipt-type-overline-weight,600);-moz-osx-font-smoothing:grayscale;-webkit-font-smoothing:antialiased;-webkit-letter-spacing:.16667em;-moz-letter-spacing:.16667em;-ms-letter-spacing:.16667em;letter-spacing:.16667em;-webkit-letter-spacing:var(--ipt-type-overline-letterSpacing,.16667em);-moz-letter-spacing:var(--ipt-type-overline-letterSpacing,.16667em);-ms-letter-spacing:var(--ipt-type-overline-letterSpacing,.16667em);letter-spacing:var(--ipt-type-overline-letterSpacing,.16667em);line-height:1rem;line-height:var(--ipt-type-overline-lineHeight,1rem);text-transform:uppercase;text-transform:var(--ipt-type-overline-textTransform,uppercase);}/*!sc*/\ndata-styled.g40[id="sc-jIZahH"]{content:"bIXmnE,"}/*!sc*/\n.eVMZHX{--ipt-base-rgb:255,255,255;--mdc-theme-ipt-base-rgb:var(--ipt-base-rgb);--ipt-base-bg:rgb(255,255,255);--mdc-theme-ipt-base-bg:var(--ipt-base-bg);--ipt-base-color:rgb(255,255,255);--mdc-theme-ipt-base-color:var(--ipt-base-color);--ipt-base-shade1-rgb:250,250,250;--mdc-theme-ipt-base-shade1-rgb:var(--ipt-base-shade1-rgb);--ipt-base-shade1-bg:rgb(250,250,250);--mdc-theme-ipt-base-shade1-bg:var(--ipt-base-shade1-bg);--ipt-base-shade1-color:rgb(250,250,250);--mdc-theme-ipt-base-shade1-color:var(--ipt-base-shade1-color);--ipt-base-shade2-rgb:240,240,240;--mdc-theme-ipt-base-shade2-rgb:var(--ipt-base-shade2-rgb);--ipt-base-shade2-bg:rgb(240,240,240);--mdc-theme-ipt-base-shade2-bg:var(--ipt-base-shade2-bg);--ipt-base-shade2-color:rgb(240,240,240);--mdc-theme-ipt-base-shade2-color:var(--ipt-base-shade2-color);--ipt-base-shade3-rgb:255,255,255;--mdc-theme-ipt-base-shade3-rgb:var(--ipt-base-shade3-rgb);--ipt-base-shade3-bg:rgb(255,255,255);--mdc-theme-ipt-base-shade3-bg:var(--ipt-base-shade3-bg);--ipt-base-shade3-color:rgb(255,255,255);--mdc-theme-ipt-base-shade3-color:var(--ipt-base-shade3-color);--ipt-on-base-rgb:0,0,0;--mdc-theme-ipt-on-base-rgb:var(--ipt-on-base-rgb);--ipt-on-base-color:rgb(0,0,0);--mdc-theme-ipt-on-base-color:var(--ipt-on-base-color);--ipt-on-base-accent1-rgb:245,197,24;--mdc-theme-ipt-on-base-accent1-rgb:var(--ipt-on-base-accent1-rgb);--ipt-on-base-accent1-color:rgb(245,197,24);--mdc-theme-ipt-on-base-accent1-color:var(--ipt-on-base-accent1-color);--ipt-on-base-accent2-rgb:14,99,190;--mdc-theme-ipt-on-base-accent2-rgb:var(--ipt-on-base-accent2-rgb);--ipt-on-base-accent2-color:rgb(14,99,190);--mdc-theme-ipt-on-base-accent2-color:var(--ipt-on-base-accent2-color);--ipt-on-base-accent3-rgb:189,36,4;--mdc-theme-ipt-on-base-accent3-rgb:var(--ipt-on-base-accent3-rgb);--ipt-on-base-accent3-color:rgb(189,36,4);--mdc-theme-ipt-on-base-accent3-color:var(--ipt-on-base-accent3-color);--ipt-on-base-accent4-rgb:0,138,0;--mdc-theme-ipt-on-base-accent4-rgb:var(--ipt-on-base-accent4-rgb);--ipt-on-base-accent4-color:rgb(0,138,0);--mdc-theme-ipt-on-base-accent4-color:var(--ipt-on-base-accent4-color);--ipt-on-base-textPrimary-opacity:0.87;--mdc-theme-ipt-on-base-textPrimary-opacity:var(--ipt-on-base-textPrimary-opacity);--ipt-on-base-textPrimary-color:rgba(0,0,0,0.87);--mdc-theme-ipt-on-base-textPrimary-color:var(--ipt-on-base-textPrimary-color);--ipt-on-base-textSecondary-opacity:0.54;--mdc-theme-ipt-on-base-textSecondary-opacity:var(--ipt-on-base-textSecondary-opacity);--ipt-on-base-textSecondary-color:rgba(0,0,0,0.54);--mdc-theme-ipt-on-base-textSecondary-color:var(--ipt-on-base-textSecondary-color);--ipt-on-base-textHint-opacity:0.38;--mdc-theme-ipt-on-base-textHint-opacity:var(--ipt-on-base-textHint-opacity);--ipt-on-base-textHint-color:rgba(0,0,0,0.38);--mdc-theme-ipt-on-base-textHint-color:var(--ipt-on-base-textHint-color);--ipt-on-base-textDisabled-opacity:0.38;--mdc-theme-ipt-on-base-textDisabled-opacity:var(--ipt-on-base-textDisabled-opacity);--ipt-on-base-textDisabled-color:rgba(0,0,0,0.38);--mdc-theme-ipt-on-base-textDisabled-color:var(--ipt-on-base-textDisabled-color);--ipt-on-base-textIcon-opacity:0.5;--mdc-theme-ipt-on-base-textIcon-opacity:var(--ipt-on-base-textIcon-opacity);--ipt-on-base-textIcon-color:rgba(0,0,0,0.5);--mdc-theme-ipt-on-base-textIcon-color:var(--ipt-on-base-textIcon-color);--ipt-base-border-opacity:0.12;--mdc-theme-ipt-base-border-opacity:var(--ipt-base-border-opacity);--ipt-base-hover-opacity:0.08;--mdc-theme-ipt-base-hover-opacity:var(--ipt-base-hover-opacity);--ipt-base-pressed-opacity:0.16;--mdc-theme-ipt-base-pressed-opacity:var(--ipt-base-pressed-opacity);--ipt-base-stripes-opacity:0.04;--mdc-theme-ipt-base-stripes-opacity:var(--ipt-base-stripes-opacity);--ipt-base-border-color:rgba(0,0,0,0.12);--mdc-theme-ipt-base-border-color:var(--ipt-base-border-color);--ipt-base-hover-color:rgba(0,0,0,0.08);--mdc-theme-ipt-base-hover-color:var(--ipt-base-hover-color);--ipt-base-pressed-color:rgba(0,0,0,0.16);--mdc-theme-ipt-base-pressed-color:var(--ipt-base-pressed-color);--ipt-base-stripes-color:rgba(0,0,0,0.04);--mdc-theme-ipt-base-stripes-color:var(--ipt-base-stripes-color);--ipt-baseAlt-rgb:0,0,0;--mdc-theme-ipt-baseAlt-rgb:var(--ipt-baseAlt-rgb);--ipt-baseAlt-bg:rgb(0,0,0);--mdc-theme-ipt-baseAlt-bg:var(--ipt-baseAlt-bg);--ipt-baseAlt-color:rgb(0,0,0);--mdc-theme-ipt-baseAlt-color:var(--ipt-baseAlt-color);--ipt-baseAlt-shade1-rgb:31,31,31;--mdc-theme-ipt-baseAlt-shade1-rgb:var(--ipt-baseAlt-shade1-rgb);--ipt-baseAlt-shade1-bg:rgb(31,31,31);--mdc-theme-ipt-baseAlt-shade1-bg:var(--ipt-baseAlt-shade1-bg);--ipt-baseAlt-shade1-color:rgb(31,31,31);--mdc-theme-ipt-baseAlt-shade1-color:var(--ipt-baseAlt-shade1-color);--ipt-baseAlt-shade2-rgb:26,26,26;--mdc-theme-ipt-baseAlt-shade2-rgb:var(--ipt-baseAlt-shade2-rgb);--ipt-baseAlt-shade2-bg:rgb(26,26,26);--mdc-theme-ipt-baseAlt-shade2-bg:var(--ipt-baseAlt-shade2-bg);--ipt-baseAlt-shade2-color:rgb(26,26,26);--mdc-theme-ipt-baseAlt-shade2-color:var(--ipt-baseAlt-shade2-color);--ipt-baseAlt-shade3-rgb:18,18,18;--mdc-theme-ipt-baseAlt-shade3-rgb:var(--ipt-baseAlt-shade3-rgb);--ipt-baseAlt-shade3-bg:rgb(18,18,18);--mdc-theme-ipt-baseAlt-shade3-bg:var(--ipt-baseAlt-shade3-bg);--ipt-baseAlt-shade3-color:rgb(18,18,18);--mdc-theme-ipt-baseAlt-shade3-color:var(--ipt-baseAlt-shade3-color);--ipt-on-baseAlt-rgb:255,255,255;--mdc-theme-ipt-on-baseAlt-rgb:var(--ipt-on-baseAlt-rgb);--ipt-on-baseAlt-color:rgb(255,255,255);--mdc-theme-ipt-on-baseAlt-color:var(--ipt-on-baseAlt-color);--ipt-on-baseAlt-accent1-rgb:245,197,24;--mdc-theme-ipt-on-baseAlt-accent1-rgb:var(--ipt-on-baseAlt-accent1-rgb);--ipt-on-baseAlt-accent1-color:rgb(245,197,24);--mdc-theme-ipt-on-baseAlt-accent1-color:var(--ipt-on-baseAlt-accent1-color);--ipt-on-baseAlt-accent2-rgb:87,153,239;--mdc-theme-ipt-on-baseAlt-accent2-rgb:var(--ipt-on-baseAlt-accent2-rgb);--ipt-on-baseAlt-accent2-color:rgb(87,153,239);--mdc-theme-ipt-on-baseAlt-accent2-color:var(--ipt-on-baseAlt-accent2-color);--ipt-on-baseAlt-accent3-rgb:251,60,60;--mdc-theme-ipt-on-baseAlt-accent3-rgb:var(--ipt-on-baseAlt-accent3-rgb);--ipt-on-baseAlt-accent3-color:rgb(251,60,60);--mdc-theme-ipt-on-baseAlt-accent3-color:var(--ipt-on-baseAlt-accent3-color);--ipt-on-baseAlt-accent4-rgb:103,173,75;--mdc-theme-ipt-on-baseAlt-accent4-rgb:var(--ipt-on-baseAlt-accent4-rgb);--ipt-on-baseAlt-accent4-color:rgb(103,173,75);--mdc-theme-ipt-on-baseAlt-accent4-color:var(--ipt-on-baseAlt-accent4-color);--ipt-on-baseAlt-textPrimary-opacity:1;--mdc-theme-ipt-on-baseAlt-textPrimary-opacity:var(--ipt-on-baseAlt-textPrimary-opacity);--ipt-on-baseAlt-textPrimary-color:rgba(255,255,255,1);--mdc-theme-ipt-on-baseAlt-textPrimary-color:var(--ipt-on-baseAlt-textPrimary-color);--ipt-on-baseAlt-textSecondary-opacity:0.7;--mdc-theme-ipt-on-baseAlt-textSecondary-opacity:var(--ipt-on-baseAlt-textSecondary-opacity);--ipt-on-baseAlt-textSecondary-color:rgba(255,255,255,0.7);--mdc-theme-ipt-on-baseAlt-textSecondary-color:var(--ipt-on-baseAlt-textSecondary-color);--ipt-on-baseAlt-textHint-opacity:0.5;--mdc-theme-ipt-on-baseAlt-textHint-opacity:var(--ipt-on-baseAlt-textHint-opacity);--ipt-on-baseAlt-textHint-color:rgba(255,255,255,0.5);--mdc-theme-ipt-on-baseAlt-textHint-color:var(--ipt-on-baseAlt-textHint-color);--ipt-on-baseAlt-textDisabled-opacity:0.5;--mdc-theme-ipt-on-baseAlt-textDisabled-opacity:var(--ipt-on-baseAlt-textDisabled-opacity);--ipt-on-baseAlt-textDisabled-color:rgba(255,255,255,0.5);--mdc-theme-ipt-on-baseAlt-textDisabled-color:var(--ipt-on-baseAlt-textDisabled-color);--ipt-on-baseAlt-textIcon-opacity:1;--mdc-theme-ipt-on-baseAlt-textIcon-opacity:var(--ipt-on-baseAlt-textIcon-opacity);--ipt-on-baseAlt-textIcon-color:rgba(255,255,255,1);--mdc-theme-ipt-on-baseAlt-textIcon-color:var(--ipt-on-baseAlt-textIcon-color);--ipt-baseAlt-border-opacity:0.2;--mdc-theme-ipt-baseAlt-border-opacity:var(--ipt-baseAlt-border-opacity);--ipt-baseAlt-hover-opacity:0.08;--mdc-theme-ipt-baseAlt-hover-opacity:var(--ipt-baseAlt-hover-opacity);--ipt-baseAlt-pressed-opacity:0.32;--mdc-theme-ipt-baseAlt-pressed-opacity:var(--ipt-baseAlt-pressed-opacity);--ipt-baseAlt-stripes-opacity:0.12;--mdc-theme-ipt-baseAlt-stripes-opacity:var(--ipt-baseAlt-stripes-opacity);--ipt-baseAlt-border-color:rgba(255,255,255,0.2);--mdc-theme-ipt-baseAlt-border-color:var(--ipt-baseAlt-border-color);--ipt-baseAlt-hover-color:rgba(255,255,255,0.08);--mdc-theme-ipt-baseAlt-hover-color:var(--ipt-baseAlt-hover-color);--ipt-baseAlt-pressed-color:rgba(255,255,255,0.32);--mdc-theme-ipt-baseAlt-pressed-color:var(--ipt-baseAlt-pressed-color);--ipt-baseAlt-stripes-color:rgba(255,255,255,0.12);--mdc-theme-ipt-baseAlt-stripes-color:var(--ipt-baseAlt-stripes-color);--ipt-accent1-rgb:245,197,24;--mdc-theme-ipt-accent1-rgb:var(--ipt-accent1-rgb);--ipt-accent1-bg:rgb(245,197,24);--mdc-theme-ipt-accent1-bg:var(--ipt-accent1-bg);--ipt-accent1-color:rgb(245,197,24);--mdc-theme-ipt-accent1-color:var(--ipt-accent1-color);--ipt-on-accent1-rgb:0,0,0;--mdc-theme-ipt-on-accent1-rgb:var(--ipt-on-accent1-rgb);--ipt-on-accent1-color:rgb(0,0,0);--mdc-theme-ipt-on-accent1-color:var(--ipt-on-accent1-color);--ipt-accent2-rgb:14,99,190;--mdc-theme-ipt-accent2-rgb:var(--ipt-accent2-rgb);--ipt-accent2-bg:rgb(14,99,190);--mdc-theme-ipt-accent2-bg:var(--ipt-accent2-bg);--ipt-accent2-color:rgb(14,99,190);--mdc-theme-ipt-accent2-color:var(--ipt-accent2-color);--ipt-on-accent2-rgb:255,255,255;--mdc-theme-ipt-on-accent2-rgb:var(--ipt-on-accent2-rgb);--ipt-on-accent2-color:rgb(255,255,255);--mdc-theme-ipt-on-accent2-color:var(--ipt-on-accent2-color);--ipt-accent3-rgb:189,36,4;--mdc-theme-ipt-accent3-rgb:var(--ipt-accent3-rgb);--ipt-accent3-bg:rgb(189,36,4);--mdc-theme-ipt-accent3-bg:var(--ipt-accent3-bg);--ipt-accent3-color:rgb(189,36,4);--mdc-theme-ipt-accent3-color:var(--ipt-accent3-color);--ipt-on-accent3-rgb:255,255,255;--mdc-theme-ipt-on-accent3-rgb:var(--ipt-on-accent3-rgb);--ipt-on-accent3-color:rgb(255,255,255);--mdc-theme-ipt-on-accent3-color:var(--ipt-on-accent3-color);--ipt-accent4-rgb:0,114,7;--mdc-theme-ipt-accent4-rgb:var(--ipt-accent4-rgb);--ipt-accent4-bg:rgb(0,114,7);--mdc-theme-ipt-accent4-bg:var(--ipt-accent4-bg);--ipt-accent4-color:rgb(0,114,7);--mdc-theme-ipt-accent4-color:var(--ipt-accent4-color);--ipt-on-accent4-rgb:255,255,255;--mdc-theme-ipt-on-accent4-rgb:var(--ipt-on-accent4-rgb);--ipt-on-accent4-color:rgb(255,255,255);--mdc-theme-ipt-on-accent4-color:var(--ipt-on-accent4-color);--ipc-pageSection-bottomMargin:.75rem;--mdc-theme-ipc-pageSection-bottomMargin:var(--ipc-pageSection-bottomMargin);--ipc-pageSection-base-bg:rgb(250,250,250);--mdc-theme-ipc-pageSection-base-bg:var(--ipc-pageSection-base-bg);--ipc-pageSection-base-rgb:250,250,250;--mdc-theme-ipc-pageSection-base-rgb:var(--ipc-pageSection-base-rgb);--ipc-pageSection-baseAlt-bg:rgb(18,18,18);--mdc-theme-ipc-pageSection-baseAlt-bg:var(--ipc-pageSection-baseAlt-bg);--ipc-pageSection-baseAlt-rgb:18,18,18;--mdc-theme-ipc-pageSection-baseAlt-rgb:var(--ipc-pageSection-baseAlt-rgb);--ipc-listCard-base-bg:rgb(255,255,255);--mdc-theme-ipc-listCard-base-bg:var(--ipc-listCard-base-bg);--ipc-listCard-base-rgb:255,255,255;--mdc-theme-ipc-listCard-base-rgb:var(--ipc-listCard-base-rgb);--ipc-listCard-baseAlt-bg:rgb(26,26,26);--mdc-theme-ipc-listCard-baseAlt-bg:var(--ipc-listCard-baseAlt-bg);--ipc-listCard-baseAlt-rgb:26,26,26;--mdc-theme-ipc-listCard-baseAlt-rgb:var(--ipc-listCard-baseAlt-rgb);--ipc-prompt-bg:rgb(31,31,31);--mdc-theme-ipc-prompt-bg:var(--ipc-prompt-bg);--ipc-prompt-rgb:31,31,31;--mdc-theme-ipc-prompt-rgb:var(--ipc-prompt-rgb);--mdc-theme-primary:var(--ipt-baseAlt-color);--mdc-theme-secondary:var(--ipt-accent1-color);--mdc-theme-background:var(--ipt-base-color);--mdc-theme-surface:var(--ipt-base-shade1-color);--mdc-theme-on-primary:var(--ipt-on-baseAlt-color);--mdc-theme-on-secondary:var(--ipt-on-accent1-color);--mdc-theme-on-surface:var(--ipt-on-base-color);--mdc-theme-text-primary-on-background:var(--ipt-on-base-textPrimary-color);--mdc-theme-text-secondary-on-background:var(--ipt-on-base-textSecondary-color);--mdc-theme-text-hint-on-background:var(--ipt-on-base-textHint-color);--mdc-theme-text-disabled-on-background:var(--ipt-on-base-textDisabled-color);--mdc-theme-text-icon-on-background:var(--ipt-on-base-textIcon-color);--mdc-theme-text-primary-on-light:var(--ipt-on-base-textPrimary-color);--mdc-theme-text-secondary-on-light:var(--ipt-on-base-textSecondary-color);--mdc-theme-text-hint-on-light:var(--ipt-on-base-textHint-color);--mdc-theme-text-disabled-on-light:var(--ipt-on-base-textDisabled-color);--mdc-theme-text-icon-on-light:var(--ipt-on-base-textIcon-color);--mdc-theme-text-primary-on-dark:var(--ipt-on-baseAlt-textPrimary-color);--mdc-theme-text-secondary-on-dark:var(--ipt-on-baseAlt-textSecondary-color);--mdc-theme-text-hint-on-dark:var(--ipt-on-baseAlt-textHint-color);--mdc-theme-text-disabled-on-dark:var(--ipt-on-baseAlt-textDisabled-color);--mdc-theme-text-icon-on-dark:var(--ipt-on-baseAlt-textIcon-color);background:rgb(18,18,18);background:var(--ipt-baseAlt-shade3-bg,rgb(18,18,18));color:rgba(255,255,255,1);color:var(--ipt-on-baseAlt-textPrimary-color,rgba(255,255,255,1));font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:1rem;font-size:var(--ipt-type-body-size,1rem);font-weight:400;font-weight:var(--ipt-type-body-weight,400);-webkit-letter-spacing:.03125em;-moz-letter-spacing:.03125em;-ms-letter-spacing:.03125em;letter-spacing:.03125em;-webkit-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-moz-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-ms-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);line-height:1.5rem;line-height:var(--ipt-type-body-lineHeight,1.5rem);text-transform:none;text-transform:var(--ipt-type-body-textTransform,none);padding:0.25rem;margin:0;position:relative;z-index:1000;min-height:3.5rem;display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;width:100%;}/*!sc*/\n.eVMZHX a{color:inherit;}/*!sc*/\n.eVMZHX .navbar__inner{display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;-webkit-box-pack:justify;-webkit-justify-content:space-between;-ms-flex-pack:justify;justify-content:space-between;width:100vw;margin:0;}/*!sc*/\n@media screen and (min-width:600px){.eVMZHX .navbar__inner{padding:0 0.75rem;}}/*!sc*/\n@media screen and (min-width:1024px){.eVMZHX .navbar__inner{width:100%;margin:0 auto;}}/*!sc*/\n.eVMZHX label{margin-bottom:0;}/*!sc*/\n.eVMZHX li{margin-bottom:0;}/*!sc*/\ndata-styled.g42[id="sc-gXmSlM"]{content:"eVMZHX,"}/*!sc*/\n.efUGki{border:1px solid rgba(var(--ipt-on-baseAlt-rgb),0.16);-webkit-order:5;-ms-flex-order:5;order:5;width:1px;height:2rem;margin:0 0.5rem;}/*!sc*/\n@media screen and (max-width:600px){.efUGki{display:none;-webkit-order:8;-ms-flex-order:8;order:8;}}/*!sc*/\ndata-styled.g43[id="sc-cCsOjp"]{content:"efUGki,"}/*!sc*/\n</style>\n<section id="imdb-bmo-navbar"><script>if(typeof uet === \'function\'){ uet(\'bb\', \'imdbHeader\', {wb: 1}); }</script><nav id="imdbHeader" class="sc-gXmSlM eVMZHX imdb-header imdb-header--react celwidget"><div id="nblogin" class="imdb-header__login-state-node"></div><div class="ipc-page-content-container ipc-page-content-container--center navbar__inner" role="presentation"><label id="imdbHeader-navDrawerOpen" class="ipc-responsive-button ipc-btn--theme-baseAlt ipc-responsive-button--transition-l ipc-btn--on-textPrimary ipc-responsive-button--single-padding sc-dIouRR eSXHLQ hamburger__icon" role="button" tabindex="0" aria-label="Open Navigation Drawer" aria-disabled="false" for="imdbHeader-navDrawer"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--menu ipc-responsive-button__icon" id="iconContext-menu" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M4 18h16c.55 0 1-.45 1-1s-.45-1-1-1H4c-.55 0-1 .45-1 1s.45 1 1 1zm0-5h16c.55 0 1-.45 1-1s-.45-1-1-1H4c-.55 0-1 .45-1 1s.45 1 1 1zM3 7c0 .55.45 1 1 1h16c.55 0 1-.45 1-1s-.45-1-1-1H4c-.55 0-1 .45-1 1z"></path></svg><span class="ipc-responsive-button__text">Menu</span></label><input type="checkbox" class="drawer__state" name="imdbHeader-navDrawer" id="imdbHeader-navDrawer" aria-hidden="true" hidden=""/><aside class="sc-crXcEl PMSTF drawer hamburger__drawer imdb-header__nav-drawer" role="presentation" data-testid="drawer"><div class="drawer__panel" role="presentation" aria-hidden="true" data-testid="panel"><div class="drawer__panelHeader" role="presentation" data-testid="panel-header"><a href="/?ref_=nv_home"><svg class="ipc-logo drawer-logo" xmlns="http://www.w3.org/2000/svg" width="98" height="56" viewBox="0 0 64 32" version="1.1"><g fill="#F5C518"><rect x="0" y="0" width="100%" height="100%" rx="4"></rect></g><g transform="translate(8.000000, 7.000000)" fill="#000000" fill-rule="nonzero"><polygon points="0 18 5 18 5 0 0 0"></polygon><path d="M15.6725178,0 L14.5534833,8.40846934 L13.8582008,3.83502426 C13.65661,2.37009263 13.4632474,1.09175121 13.278113,0 L7,0 L7,18 L11.2416347,18 L11.2580911,6.11380679 L13.0436094,18 L16.0633571,18 L17.7583653,5.8517865 L17.7707076,18 L22,18 L22,0 L15.6725178,0 Z"></path><path d="M24,18 L24,0 L31.8045586,0 C33.5693522,0 35,1.41994415 35,3.17660424 L35,14.8233958 C35,16.5777858 33.5716617,18 31.8045586,18 L24,18 Z M29.8322479,3.2395236 C29.6339219,3.13233348 29.2545158,3.08072342 28.7026524,3.08072342 L28.7026524,14.8914865 C29.4312846,14.8914865 29.8796736,14.7604764 30.0478195,14.4865461 C30.2159654,14.2165858 30.3021941,13.486105 30.3021941,12.2871637 L30.3021941,5.3078959 C30.3021941,4.49404499 30.272014,3.97397442 30.2159654,3.74371416 C30.1599168,3.5134539 30.0348852,3.34671372 29.8322479,3.2395236 Z"></path><path d="M44.4299079,4.50685823 L44.749518,4.50685823 C46.5447098,4.50685823 48,5.91267586 48,7.64486762 L48,14.8619906 C48,16.5950653 46.5451816,18 44.749518,18 L44.4299079,18 C43.3314617,18 42.3602746,17.4736618 41.7718697,16.6682739 L41.4838962,17.7687785 L37,17.7687785 L37,0 L41.7843263,0 L41.7843263,5.78053556 C42.4024982,5.01015739 43.3551514,4.50685823 44.4299079,4.50685823 Z M43.4055679,13.2842155 L43.4055679,9.01907814 C43.4055679,8.31433946 43.3603268,7.85185468 43.2660746,7.63896485 C43.1718224,7.42607505 42.7955881,7.2893916 42.5316822,7.2893916 C42.267776,7.2893916 41.8607934,7.40047379 41.7816216,7.58767002 L41.7816216,9.01907814 L41.7816216,13.4207851 L41.7816216,14.8074788 C41.8721037,15.0130276 42.2602358,15.1274059 42.5316822,15.1274059 C42.8031285,15.1274059 43.1982131,15.0166981 43.281155,14.8074788 C43.3640968,14.5982595 43.4055679,14.0880581 43.4055679,13.2842155 Z"></path></g></svg></a><label class="ipc-icon-button drawer__panelHeaderClose ipc-icon-button--baseAlt ipc-icon-button--onBase" title="Close Navigation Drawer" role="button" tabindex="0" aria-label="Close Navigation Drawer" aria-disabled="false" for="imdbHeader-navDrawer"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--clear" id="iconContext-clear" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M18.3 5.71a.996.996 0 0 0-1.41 0L12 10.59 7.11 5.7A.996.996 0 1 0 5.7 7.11L10.59 12 5.7 16.89a.996.996 0 1 0 1.41 1.41L12 13.41l4.89 4.89a.996.996 0 1 0 1.41-1.41L13.41 12l4.89-4.89c.38-.38.38-1.02 0-1.4z"></path></svg></label></div><div class="drawer__panelContent" role="presentation" data-testid="panel-content"><div role="presentation" class="sc-fEOsli iYlngk navlcl"><div class="sc-breuTD cqozJK navlinkcat" data-testid="nav-link-category" role="presentation"><input type="radio" class="navlinkcat__state" name="nav-categories-list" id="nav-link-categories-mov" tabindex="-1" data-category-id="mov" hidden="" aria-hidden="true"/><span class="navlinkcat__targetWrapper"><label role="button" aria-label="Expand Movies Nav Links" class="navlinkcat__item" tabindex="0" for="nav-link-categories-mov" data-testid="category-expando"><span class="navlinkcat__itemIcon"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--movie" id="iconContext-movie" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M18 4v1h-2V4c0-.55-.45-1-1-1H9c-.55 0-1 .45-1 1v1H6V4c0-.55-.45-1-1-1s-1 .45-1 1v16c0 .55.45 1 1 1s1-.45 1-1v-1h2v1c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-1h2v1c0 .55.45 1 1 1s1-.45 1-1V4c0-.55-.45-1-1-1s-1 .45-1 1zM8 17H6v-2h2v2zm0-4H6v-2h2v2zm0-4H6V7h2v2zm10 8h-2v-2h2v2zm0-4h-2v-2h2v2zm0-4h-2V7h2v2z"></path></svg></span><span class="navlinkcat__itemTitle">Movies</span><span class="navlinkcat__itemChevron"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--chevron-right" id="iconContext-chevron-right" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M9.29 6.71a.996.996 0 0 0 0 1.41L13.17 12l-3.88 3.88a.996.996 0 1 0 1.41 1.41l4.59-4.59a.996.996 0 0 0 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"></path></svg></span></label><div class="navlinkcat__listContainer" aria-hidden="true" aria-expanded="false" data-testid="list-container"><div class="navlinkcat__listContainerInner" role="presentation"><ul class="ipc-list navlinkcat__list ipc-list--baseAlt" role="menu" aria-orientation="vertical"><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="https://www.imdb.com/calendar/?ref_=nv_mv_cal" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Release Calendar</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/chart/top/?ref_=nv_mv_250" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Top 250 Movies</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/chart/moviemeter/?ref_=nv_mv_mpm" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Most Popular Movies</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/feature/genre/?ref_=nv_ch_gr" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Browse Movies by Genre</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/chart/boxoffice/?ref_=nv_ch_cht" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Top Box Office</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/showtimes/?ref_=nv_mv_sh" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Showtimes &amp; Tickets</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/news/movie/?ref_=nv_nw_mv" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Movie News</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/india/toprated/?ref_=nv_mv_in" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">India Movie Spotlight</span></a></ul></div></div></span></div><div data-testid="grouped-link-category" class="sc-ksZaOG gSOLIo"><div class="sc-breuTD cqozJK navlinkcat sc-fnykZs SyRwP" data-testid="nav-link-category" role="presentation"><input type="radio" class="navlinkcat__state" name="nav-categories-list" id="nav-link-categories-tvshows" tabindex="-1" data-category-id="tvshows" hidden="" aria-hidden="true"/><span class="navlinkcat__targetWrapper"><label role="button" aria-label="Expand TV Shows Nav Links" class="navlinkcat__item" tabindex="0" for="nav-link-categories-tvshows" data-testid="category-expando"><span class="navlinkcat__itemIcon"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--television" id="iconContext-television" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M21 3H3c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h5v1c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-1h5c1.1 0 1.99-.9 1.99-2L23 5a2 2 0 0 0-2-2zm-1 14H4c-.55 0-1-.45-1-1V6c0-.55.45-1 1-1h16c.55 0 1 .45 1 1v10c0 .55-.45 1-1 1z"></path></svg></span><span class="navlinkcat__itemTitle">TV Shows</span><span class="navlinkcat__itemChevron"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--chevron-right" id="iconContext-chevron-right" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M9.29 6.71a.996.996 0 0 0 0 1.41L13.17 12l-3.88 3.88a.996.996 0 1 0 1.41 1.41l4.59-4.59a.996.996 0 0 0 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"></path></svg></span></label><div class="navlinkcat__listContainer" aria-hidden="true" aria-expanded="false" data-testid="list-container"><div class="navlinkcat__listContainerInner" role="presentation"><ul class="ipc-list navlinkcat__list ipc-list--baseAlt" role="menu" aria-orientation="vertical"><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/whats-on-tv/?ref_=nv_tv_ontv" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">What&#x27;s on TV &amp; Streaming</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/chart/toptv/?ref_=nv_tvv_250" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Top 250 TV Shows</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/chart/tvmeter/?ref_=nv_tvv_mptv" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Most Popular TV Shows</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/feature/genre/" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Browse TV Shows by Genre</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/news/tv/?ref_=nv_nw_tv" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">TV News</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/india/tv?ref_=nv_tv_in" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">India TV Spotlight</span></a></ul></div></div></span></div><div class="sc-breuTD cqozJK navlinkcat sc-fnykZs SyRwP" data-testid="nav-link-category" role="presentation"><input type="radio" class="navlinkcat__state" name="nav-categories-list" id="nav-link-categories-video" tabindex="-1" data-category-id="video" hidden="" aria-hidden="true"/><span class="navlinkcat__targetWrapper"><label role="button" aria-label="Expand Watch Nav Links" class="navlinkcat__item" tabindex="0" for="nav-link-categories-video" data-testid="category-expando"><span class="navlinkcat__itemIcon"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--video-library" id="iconContext-video-library" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M3 6c-.55 0-1 .45-1 1v13c0 1.1.9 2 2 2h13c.55 0 1-.45 1-1s-.45-1-1-1H5c-.55 0-1-.45-1-1V7c0-.55-.45-1-1-1zm17-4H8c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm-8 12.5v-9l5.47 4.1c.27.2.27.6 0 .8L12 14.5z"></path></svg></span><span class="navlinkcat__itemTitle">Watch</span><span class="navlinkcat__itemChevron"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--chevron-right" id="iconContext-chevron-right" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M9.29 6.71a.996.996 0 0 0 0 1.41L13.17 12l-3.88 3.88a.996.996 0 1 0 1.41 1.41l4.59-4.59a.996.996 0 0 0 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"></path></svg></span></label><div class="navlinkcat__listContainer" aria-hidden="true" aria-expanded="false" data-testid="list-container"><div class="navlinkcat__listContainerInner" role="presentation"><ul class="ipc-list navlinkcat__list ipc-list--baseAlt" role="menu" aria-orientation="vertical"><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/what-to-watch/?ref_=nv_watch" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">What to Watch</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/trailers/?ref_=nv_mv_tr" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Latest Trailers</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/originals/?ref_=nv_sf_ori" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">IMDb Originals</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/imdbpicks/?ref_=nv_pi" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">IMDb Picks</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/podcasts/?ref_=nv_pod" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">IMDb Podcasts</span></a></ul></div></div></span></div></div><div class="sc-breuTD cqozJK navlinkcat" data-testid="nav-link-category" role="presentation"><input type="radio" class="navlinkcat__state" name="nav-categories-list" id="nav-link-categories-awards" tabindex="-1" data-category-id="awards" hidden="" aria-hidden="true"/><span class="navlinkcat__targetWrapper"><label role="button" aria-label="Expand Awards &amp; Events Nav Links" class="navlinkcat__item" tabindex="0" for="nav-link-categories-awards" data-testid="category-expando"><span class="navlinkcat__itemIcon"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--star-circle-filled" id="iconContext-star-circle-filled" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M11.99 2C6.47 2 2 6.48 2 12s4.47 10 9.99 10C17.52 22 22 17.52 22 12S17.52 2 11.99 2zm3.23 15.39L12 15.45l-3.22 1.94a.502.502 0 0 1-.75-.54l.85-3.66-2.83-2.45a.505.505 0 0 1 .29-.88l3.74-.32 1.46-3.45c.17-.41.75-.41.92 0l1.46 3.44 3.74.32a.5.5 0 0 1 .28.88l-2.83 2.45.85 3.67c.1.43-.36.77-.74.54z"></path></svg></span><span class="navlinkcat__itemTitle">Awards &amp; Events</span><span class="navlinkcat__itemChevron"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--chevron-right" id="iconContext-chevron-right" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M9.29 6.71a.996.996 0 0 0 0 1.41L13.17 12l-3.88 3.88a.996.996 0 1 0 1.41 1.41l4.59-4.59a.996.996 0 0 0 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"></path></svg></span></label><div class="navlinkcat__listContainer" aria-hidden="true" aria-expanded="false" data-testid="list-container"><div class="navlinkcat__listContainerInner" role="presentation"><ul class="ipc-list navlinkcat__list ipc-list--baseAlt" role="menu" aria-orientation="vertical"><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/oscars/?ref_=nv_ev_acd" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Oscars</span></a><a role="menuitem" class="ipc-list__item nav-link nav-link--hideL nav-link--hideXL sc-evZas nMWbL ipc-list__item--indent-one" href="https://m.imdb.com/feature/bestpicture/?ref_=nv_ch_osc" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Best Picture Winners</span></a><a role="menuitem" class="ipc-list__item nav-link nav-link--hideXS nav-link--hideS nav-link--hideM sc-evZas nMWbL ipc-list__item--indent-one" href="https://www.imdb.com/search/title/?count=100&amp;groups=oscar_best_picture_winners&amp;sort=year%2Cdesc&amp;ref_=nv_ch_osc" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Best Picture Winners</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/emmys/?ref_=nv_ev_rte" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Emmys</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/starmeterawards/?ref_=nv_ev_sma" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">STARmeter Awards</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/comic-con/?ref_=nv_ev_comic" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">San Diego Comic-Con</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/nycc/?ref_=nv_ev_nycc" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">New York Comic-Con</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/sundance/?ref_=nv_ev_sun" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Sundance Film Festival</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/toronto/?ref_=nv_ev_tor" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Toronto Int&#x27;l Film Festival</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/awards-central/?ref_=nv_ev_awrd" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Awards Central</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/festival-central/?ref_=nv_ev_fc" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Festival Central</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/event/all/?ref_=nv_ev_all" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">All Events</span></a></ul></div></div></span></div><div class="sc-breuTD cqozJK navlinkcat noMarginItem" data-testid="nav-link-category" role="presentation"><input type="radio" class="navlinkcat__state" name="nav-categories-list" id="nav-link-categories-celebs" tabindex="-1" data-category-id="celebs" hidden="" aria-hidden="true"/><span class="navlinkcat__targetWrapper"><label role="button" aria-label="Expand Celebs Nav Links" class="navlinkcat__item" tabindex="0" for="nav-link-categories-celebs" data-testid="category-expando"><span class="navlinkcat__itemIcon"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--people" id="iconContext-people" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M16 11c1.66 0 2.99-1.34 2.99-3S17.66 5 16 5s-3 1.34-3 3 1.34 3 3 3zm-8 0c1.66 0 2.99-1.34 2.99-3S9.66 5 8 5 5 6.34 5 8s1.34 3 3 3zm0 2c-2.33 0-7 1.17-7 3.5V18c0 .55.45 1 1 1h12c.55 0 1-.45 1-1v-1.5c0-2.33-4.67-3.5-7-3.5zm8 0c-.29 0-.62.02-.97.05.02.01.03.03.04.04 1.14.83 1.93 1.94 1.93 3.41V18c0 .35-.07.69-.18 1H22c.55 0 1-.45 1-1v-1.5c0-2.33-4.67-3.5-7-3.5z"></path></svg></span><span class="navlinkcat__itemTitle">Celebs</span><span class="navlinkcat__itemChevron"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--chevron-right" id="iconContext-chevron-right" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M9.29 6.71a.996.996 0 0 0 0 1.41L13.17 12l-3.88 3.88a.996.996 0 1 0 1.41 1.41l4.59-4.59a.996.996 0 0 0 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"></path></svg></span></label><div class="navlinkcat__listContainer" aria-hidden="true" aria-expanded="false" data-testid="list-container"><div class="navlinkcat__listContainerInner" role="presentation"><ul class="ipc-list navlinkcat__list ipc-list--baseAlt" role="menu" aria-orientation="vertical"><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/feature/bornondate/?ref_=nv_cel_brn" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Born Today</span></a><a role="menuitem" class="ipc-list__item nav-link nav-link--hideL nav-link--hideXL sc-evZas nMWbL ipc-list__item--indent-one" href="https://m.imdb.com/chart/starmeter/?ref_=nv_cel_brn" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Most Popular Celebs</span></a><a role="menuitem" class="ipc-list__item nav-link nav-link--hideXS nav-link--hideS nav-link--hideM sc-evZas nMWbL ipc-list__item--indent-one" href="https://www.imdb.com/search/name/?match_all=true&amp;ref_=nv_cel_m" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Most Popular Celebs</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/news/celebrity/?ref_=nv_cel_nw" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Celebrity News</span></a></ul></div></div></span></div><div data-testid="nav-link-category" class="sc-hAZoDl OzViy"></div><div class="sc-breuTD cqozJK navlinkcat noMarginItem" data-testid="nav-link-category" role="presentation"><input type="radio" class="navlinkcat__state" name="nav-categories-list" id="nav-link-categories-comm" tabindex="-1" data-category-id="comm" hidden="" aria-hidden="true"/><span class="navlinkcat__targetWrapper"><label role="button" aria-label="Expand Community Nav Links" class="navlinkcat__item" tabindex="0" for="nav-link-categories-comm" data-testid="category-expando"><span class="navlinkcat__itemIcon"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--earth" id="iconContext-earth" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.95-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z"></path></svg></span><span class="navlinkcat__itemTitle">Community</span><span class="navlinkcat__itemChevron"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--chevron-right" id="iconContext-chevron-right" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M9.29 6.71a.996.996 0 0 0 0 1.41L13.17 12l-3.88 3.88a.996.996 0 1 0 1.41 1.41l4.59-4.59a.996.996 0 0 0 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"></path></svg></span></label><div class="navlinkcat__listContainer" aria-hidden="true" aria-expanded="false" data-testid="list-container"><div class="navlinkcat__listContainerInner" role="presentation"><ul class="ipc-list navlinkcat__list ipc-list--baseAlt" role="menu" aria-orientation="vertical"><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="https://help.imdb.com/imdb?ref_=cons_nb_hlp" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Help Center</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="https://contribute.imdb.com/czone?ref_=nv_cm_cz" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Contributor Zone</span></a><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL ipc-list__item--indent-one" href="/poll/?ref_=nv_cm_pl" tabindex="-1" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Polls</span></a></ul></div></div></span></div><a role="menuitem" class="ipc-list__item nav-link sc-evZas nMWbL navlcl__proLink" href="https://pro.imdb.com?ref_=cons_nb_hm&amp;rf=cons_nb_hm" target="_blank" aria-label="Go To IMDb Pro" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><div class="sc-bjUoiL bpTVou navlcl__logoNavLink"><svg class="ipc-logo" width="52" height="14" viewBox="0 0 52 14" xmlns="http://www.w3.org/2000/svg" version="1.1"><g fill="currentColor"><rect x="0" y="1" width="3.21" height="12.34"></rect><path d="M10,1 L9.3,6.76 L8.84,3.63 C8.7,2.62 8.58,1.75 8.45,1 L4.3,1 L4.3,13.34 L7.11,13.34 L7.11,5.19 L8.3,13.34 L10.3,13.34 L11.42,5 L11.42,13.33 L14.22,13.33 L14.22,1 L10,1 Z"></path><path d="M19.24,3.22 C19.3711159,3.29185219 19.4602235,3.42180078 19.48,3.57 C19.5340993,3.92393477 19.554191,4.28223587 19.54,4.64 L19.54,9.42 C19.578852,9.92887392 19.5246327,10.4405682 19.38,10.93 C19.27,11.12 18.99,11.21 18.53,11.21 L18.53,3.11 C18.7718735,3.09406934 19.0142863,3.13162626 19.24,3.22 Z M19.24,13.34 C19.8163127,13.3574057 20.3928505,13.3138302 20.96,13.21 C21.3245396,13.1481159 21.6680909,12.9969533 21.96,12.77 C22.2288287,12.5438006 22.4209712,12.2398661 22.51,11.9 C22.643288,11.1679419 22.6969338,10.4236056 22.67,9.68 L22.67,5.34 C22.6662002,4.55669241 22.6060449,3.77467335 22.49,3 C22.43037,2.59841431 22.260779,2.22116094 22,1.91 C21.6636187,1.56093667 21.2326608,1.317654 20.76,1.21 C19.7709421,1.02848785 18.7647002,0.958050915 17.76,1 L15.32,1 L15.32,13.34 L19.24,13.34 Z"></path><path d="M27.86,10.34 C27.8769902,10.7218086 27.8501483,11.1043064 27.78,11.48 C27.72,11.63 27.46,11.71 27.26,11.71 C27.0954951,11.7299271 26.9386363,11.6349863 26.88,11.48 C26.7930212,11.1542289 26.7592527,10.8165437 26.78,10.48 L26.78,7.18 C26.7626076,6.84408875 26.7929089,6.50740774 26.87,6.18 C26.9317534,6.03447231 27.0833938,5.94840616 27.24,5.97 C27.43,5.97 27.7,6.05 27.76,6.21 C27.8468064,6.53580251 27.8805721,6.87345964 27.86,7.21 L27.86,10.34 Z M23.7,1 L23.7,13.34 L26.58,13.34 L26.78,12.55 C27.0112432,12.8467609 27.3048209,13.0891332 27.64,13.26 C28.0022345,13.4198442 28.394069,13.5016184 28.79,13.5 C29.2588971,13.515288 29.7196211,13.3746089 30.1,13.1 C30.4399329,12.8800058 30.6913549,12.5471372 30.81,12.16 C30.9423503,11.6167622 31.0061799,11.0590937 31,10.5 L31,7 C31.0087531,6.51279482 30.9920637,6.02546488 30.95,5.54 C30.904474,5.28996521 30.801805,5.05382649 30.65,4.85 C30.4742549,4.59691259 30.2270668,4.40194735 29.94,4.29 C29.5869438,4.15031408 29.2096076,4.08232558 28.83,4.09 C28.4361722,4.08961884 28.0458787,4.16428368 27.68,4.31 C27.3513666,4.46911893 27.0587137,4.693713 26.82,4.97 L26.82,1 L23.7,1 Z"></path><path d="M32.13,1 L35.32,1 C35.9925574,0.978531332 36.6650118,1.04577677 37.32,1.2 C37.717112,1.29759578 38.0801182,1.50157071 38.37,1.79 C38.6060895,2.05302496 38.7682605,2.37391646 38.84,2.72 C38.935586,3.27463823 38.9757837,3.8374068 38.96,4.4 L38.96,5.46 C38.9916226,6.03689533 38.9100917,6.61440551 38.72,7.16 C38.5402933,7.53432344 38.2260614,7.82713037 37.84,7.98 C37.3049997,8.18709035 36.7332458,8.28238268 36.16,8.26 L35.31,8.26 L35.31,13.16 L32.13,13.16 L32.13,1 Z M35.29,3.08 L35.29,6.18 L35.53,6.18 C35.7515781,6.20532753 35.9725786,6.12797738 36.13,5.97 C36.2717869,5.69610033 36.3308522,5.38687568 36.3,5.08 L36.3,4.08 C36.3390022,3.79579475 36.2713114,3.5072181 36.11,3.27 C35.8671804,3.11299554 35.5771259,3.04578777 35.29,3.08 Z"></path><path d="M42,4.36 L41.89,5.52 C42.28,4.69 43.67,4.42 44.41,4.37 L43.6,7.3 C43.2290559,7.27725357 42.8582004,7.34593052 42.52,7.5 C42.3057075,7.61238438 42.1519927,7.81367763 42.1,8.05 C42.0178205,8.59259006 41.9843538,9.14144496 42,9.69 L42,13.16 L39.34,13.16 L39.34,4.36 L42,4.36 Z"></path><path d="M51.63,9.71 C51.6472876,10.3265292 51.6003682,10.9431837 51.49,11.55 C51.376862,11.9620426 51.1639158,12.3398504 50.87,12.65 C50.5352227,13.001529 50.1148049,13.2599826 49.65,13.4 C49.0994264,13.5686585 48.5257464,13.6496486 47.95,13.64 C47.3333389,13.6524659 46.7178074,13.5818311 46.12,13.43 C45.6996896,13.322764 45.3140099,13.1092627 45,12.81 C44.7275808,12.5275876 44.5254637,12.1850161 44.41,11.81 C44.2627681,11.2181509 44.1921903,10.6098373 44.2,10 L44.2,7.64 C44.1691064,6.9584837 44.2780071,6.27785447 44.52,5.64 C44.7547114,5.12751365 45.1616363,4.71351186 45.67,4.47 C46.3337168,4.13941646 47.0688388,3.97796445 47.81,4 C48.4454888,3.98667568 49.0783958,4.08482705 49.68,4.29 C50.1352004,4.42444561 50.5506052,4.66819552 50.89,5 C51.1535526,5.26601188 51.3550281,5.58700663 51.48,5.94 C51.6001358,6.42708696 51.6506379,6.92874119 51.63,7.43 L51.63,9.71 Z M48.39,6.73 C48.412199,6.42705368 48.3817488,6.12255154 48.3,5.83 C48.2091142,5.71223121 48.0687606,5.64325757 47.92,5.64325757 C47.7712394,5.64325757 47.6308858,5.71223121 47.54,5.83 C47.447616,6.12046452 47.4136298,6.42634058 47.44,6.73 L47.44,10.93 C47.4168299,11.2204468 47.4508034,11.5126191 47.54,11.79 C47.609766,11.9270995 47.7570827,12.0067302 47.91,11.99 C48.0639216,12.0108082 48.2159732,11.9406305 48.3,11.81 C48.3790864,11.5546009 48.4096133,11.2866434 48.39,11.02 L48.39,6.73 Z"></path></g></svg><div class="sc-idiyUo cUVKHb">For Industry Professionals</div></div></span><span class="ipc-list-item__icon ipc-list-item__icon--post" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--launch" id="iconContext-launch" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M16 16.667H8A.669.669 0 0 1 7.333 16V8c0-.367.3-.667.667-.667h3.333c.367 0 .667-.3.667-.666C12 6.3 11.7 6 11.333 6h-4C6.593 6 6 6.6 6 7.333v9.334C6 17.4 6.6 18 7.333 18h9.334C17.4 18 18 17.4 18 16.667v-4c0-.367-.3-.667-.667-.667-.366 0-.666.3-.666.667V16c0 .367-.3.667-.667.667zm-2.667-10c0 .366.3.666.667.666h1.727L9.64 13.42a.664.664 0 1 0 .94.94l6.087-6.087V10c0 .367.3.667.666.667.367 0 .667-.3.667-.667V6h-4c-.367 0-.667.3-.667.667z"></path></svg></span></a></div></div></div><label class="drawer__backdrop" data-testid="backdrop" role="button" for="imdbHeader-navDrawer" tabindex="0" aria-hidden="true" aria-label="Close Navigation Drawer"></label></aside><a class="sc-bczRLJ hKcFfj imdb-header__logo-link" id="home_img_holder" href="/?ref_=nv_home" aria-label="Home"><svg id="home_img" class="ipc-logo" xmlns="http://www.w3.org/2000/svg" width="64" height="32" viewBox="0 0 64 32" version="1.1"><g fill="#F5C518"><rect x="0" y="0" width="100%" height="100%" rx="4"></rect></g><g transform="translate(8.000000, 7.000000)" fill="#000000" fill-rule="nonzero"><polygon points="0 18 5 18 5 0 0 0"></polygon><path d="M15.6725178,0 L14.5534833,8.40846934 L13.8582008,3.83502426 C13.65661,2.37009263 13.4632474,1.09175121 13.278113,0 L7,0 L7,18 L11.2416347,18 L11.2580911,6.11380679 L13.0436094,18 L16.0633571,18 L17.7583653,5.8517865 L17.7707076,18 L22,18 L22,0 L15.6725178,0 Z"></path><path d="M24,18 L24,0 L31.8045586,0 C33.5693522,0 35,1.41994415 35,3.17660424 L35,14.8233958 C35,16.5777858 33.5716617,18 31.8045586,18 L24,18 Z M29.8322479,3.2395236 C29.6339219,3.13233348 29.2545158,3.08072342 28.7026524,3.08072342 L28.7026524,14.8914865 C29.4312846,14.8914865 29.8796736,14.7604764 30.0478195,14.4865461 C30.2159654,14.2165858 30.3021941,13.486105 30.3021941,12.2871637 L30.3021941,5.3078959 C30.3021941,4.49404499 30.272014,3.97397442 30.2159654,3.74371416 C30.1599168,3.5134539 30.0348852,3.34671372 29.8322479,3.2395236 Z"></path><path d="M44.4299079,4.50685823 L44.749518,4.50685823 C46.5447098,4.50685823 48,5.91267586 48,7.64486762 L48,14.8619906 C48,16.5950653 46.5451816,18 44.749518,18 L44.4299079,18 C43.3314617,18 42.3602746,17.4736618 41.7718697,16.6682739 L41.4838962,17.7687785 L37,17.7687785 L37,0 L41.7843263,0 L41.7843263,5.78053556 C42.4024982,5.01015739 43.3551514,4.50685823 44.4299079,4.50685823 Z M43.4055679,13.2842155 L43.4055679,9.01907814 C43.4055679,8.31433946 43.3603268,7.85185468 43.2660746,7.63896485 C43.1718224,7.42607505 42.7955881,7.2893916 42.5316822,7.2893916 C42.267776,7.2893916 41.8607934,7.40047379 41.7816216,7.58767002 L41.7816216,9.01907814 L41.7816216,13.4207851 L41.7816216,14.8074788 C41.8721037,15.0130276 42.2602358,15.1274059 42.5316822,15.1274059 C42.8031285,15.1274059 43.1982131,15.0166981 43.281155,14.8074788 C43.3640968,14.5982595 43.4055679,14.0880581 43.4055679,13.2842155 Z"></path></g></svg></a><input type="checkbox" class="sc-iqcoie iKpzZH imdb-header-search__state sbar__mobileSearchStateToggle" id="navSearch-searchState" name="navSearch-searchState" aria-hidden="true" hidden=""/><div id="suggestion-search-container" class="nav-search__search-container sbar sc-jqUVSM iJNbPT"><form id="nav-search-form" name="nav-search-form" method="get" action="/find" class="searchform imdb-header__search-form sc-papXJ exojnx" role="search"><div class="sc-ftvSup cumdHr"><div class="sc-iBkjds iosfhR navbar__flyout--breakpoint-m navbar__flyout--positionLeft"><label class="ipc-button ipc-button--single-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-base ipc-button--theme-base ipc-button--on-textPrimary ipc-text-button navbar__flyout__text-button-after-mobile searchCatSelector__opener searchform__categories nav-search-form__categories" role="button" tabindex="0" aria-label="All" aria-disabled="false" for="navbar-search-category-select"><div class="ipc-button__text">All<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--arrow-drop-down navbar__flyout__button-pointer" id="iconContext-arrow-drop-down" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M8.71 11.71l2.59 2.59c.39.39 1.02.39 1.41 0l2.59-2.59c.63-.63.18-1.71-.71-1.71H9.41c-.89 0-1.33 1.08-.7 1.71z"></path></svg></div></label><input type="checkbox" class="ipc-menu__focused-state" id="navbar-search-category-select" name="navbar-search-category-select" hidden="" tabindex="-1" aria-hidden="true"/><div class="ipc-menu mdc-menu ipc-menu--not-initialized ipc-menu--on-baseAlt ipc-menu--anchored ipc-menu--with-checkbox ipc-menu--expand-from-top-left navbar__flyout--menu" data-menu-id="navbar-search-category-select" role="presentation"><div class="ipc-menu__items mdc-menu__items" role="presentation"><span id="navbar-search-category-select-contents"><ul class="ipc-list searchCatSelector ipc-list--baseAlt" role="menu" aria-orientation="vertical"><li role="menuitem" class="ipc-list__item searchCatSelector__item searchCatSelector__itemSelected" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--search searchCatSelector__itemIcon" id="iconContext-search" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M15.5 14h-.79l-.28-.27a6.5 6.5 0 0 0 1.48-5.34c-.47-2.78-2.79-5-5.59-5.34a6.505 6.505 0 0 0-7.27 7.27c.34 2.8 2.56 5.12 5.34 5.59a6.5 6.5 0 0 0 5.34-1.48l.27.28v.79l4.25 4.25c.41.41 1.08.41 1.49 0 .41-.41.41-1.08 0-1.49L15.5 14zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"></path></svg>All</span></li><li role="menuitem" class="ipc-list__item searchCatSelector__item" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--movie searchCatSelector__itemIcon" id="iconContext-movie" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M18 4v1h-2V4c0-.55-.45-1-1-1H9c-.55 0-1 .45-1 1v1H6V4c0-.55-.45-1-1-1s-1 .45-1 1v16c0 .55.45 1 1 1s1-.45 1-1v-1h2v1c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-1h2v1c0 .55.45 1 1 1s1-.45 1-1V4c0-.55-.45-1-1-1s-1 .45-1 1zM8 17H6v-2h2v2zm0-4H6v-2h2v2zm0-4H6V7h2v2zm10 8h-2v-2h2v2zm0-4h-2v-2h2v2zm0-4h-2V7h2v2z"></path></svg>Titles</span></li><li role="menuitem" class="ipc-list__item searchCatSelector__item" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--television searchCatSelector__itemIcon" id="iconContext-television" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M21 3H3c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h5v1c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-1h5c1.1 0 1.99-.9 1.99-2L23 5a2 2 0 0 0-2-2zm-1 14H4c-.55 0-1-.45-1-1V6c0-.55.45-1 1-1h16c.55 0 1 .45 1 1v10c0 .55-.45 1-1 1z"></path></svg>TV Episodes</span></li><li role="menuitem" class="ipc-list__item searchCatSelector__item" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--people searchCatSelector__itemIcon" id="iconContext-people" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M16 11c1.66 0 2.99-1.34 2.99-3S17.66 5 16 5s-3 1.34-3 3 1.34 3 3 3zm-8 0c1.66 0 2.99-1.34 2.99-3S9.66 5 8 5 5 6.34 5 8s1.34 3 3 3zm0 2c-2.33 0-7 1.17-7 3.5V18c0 .55.45 1 1 1h12c.55 0 1-.45 1-1v-1.5c0-2.33-4.67-3.5-7-3.5zm8 0c-.29 0-.62.02-.97.05.02.01.03.03.04.04 1.14.83 1.93 1.94 1.93 3.41V18c0 .35-.07.69-.18 1H22c.55 0 1-.45 1-1v-1.5c0-2.33-4.67-3.5-7-3.5z"></path></svg>Celebs</span></li><li role="menuitem" class="ipc-list__item searchCatSelector__item" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--business searchCatSelector__itemIcon" id="iconContext-business" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 7V5c0-1.1-.9-2-2-2H4c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V9c0-1.1-.9-2-2-2h-8zM6 19H4v-2h2v2zm0-4H4v-2h2v2zm0-4H4V9h2v2zm0-4H4V5h2v2zm4 12H8v-2h2v2zm0-4H8v-2h2v2zm0-4H8V9h2v2zm0-4H8V5h2v2zm9 12h-7v-2h2v-2h-2v-2h2v-2h-2V9h7c.55 0 1 .45 1 1v8c0 .55-.45 1-1 1zm-1-8h-2v2h2v-2zm0 4h-2v2h2v-2z"></path></svg>Companies</span></li><li role="menuitem" class="ipc-list__item searchCatSelector__item" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--label searchCatSelector__itemIcon" id="iconContext-label" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M17.63 5.84C17.27 5.33 16.67 5 16 5L5 5.01C3.9 5.01 3 5.9 3 7v10c0 1.1.9 1.99 2 1.99L16 19c.67 0 1.27-.33 1.63-.84l3.96-5.58a.99.99 0 0 0 0-1.16l-3.96-5.58z"></path></svg>Keywords</span></li><li role="separator" class="ipc-list-divider"></li><a role="menuitem" class="ipc-list__item searchCatSelector__item" href="https://www.imdb.com/search/" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--find-in-page searchCatSelector__itemIcon" id="iconContext-find-in-page" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M20 19.59V8.83c0-.53-.21-1.04-.59-1.41l-4.83-4.83c-.37-.38-.88-.59-1.41-.59H6c-1.1 0-1.99.9-1.99 2L4 20c0 1.1.89 2 1.99 2H18c.45 0 .85-.15 1.19-.4l-4.43-4.43c-.86.56-1.89.88-3 .82-2.37-.11-4.4-1.96-4.72-4.31a5.013 5.013 0 0 1 5.83-5.61c1.95.33 3.57 1.85 4 3.78.33 1.46.01 2.82-.7 3.9L20 19.59zM9 13c0 1.66 1.34 3 3 3s3-1.34 3-3-1.34-3-3-3-3 1.34-3 3z"></path></svg>Advanced Search</span><span class="ipc-list-item__icon ipc-list-item__icon--post" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--chevron-right" id="iconContext-chevron-right" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M9.29 6.71a.996.996 0 0 0 0 1.41L13.17 12l-3.88 3.88a.996.996 0 1 0 1.41 1.41l4.59-4.59a.996.996 0 0 0 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"></path></svg></span></a></ul></span></div></div></div></div><div class="sc-gKXOVf lbYfkb searchform__inputContainer"><div role="combobox" aria-haspopup="listbox" aria-owns="react-autowhatever-1" aria-expanded="false" class="react-autosuggest__container"><input type="text" value="" autoComplete="off" aria-autocomplete="list" aria-controls="react-autowhatever-1" class="imdb-header-search__input searchTypeahead__input react-autosuggest__input" id="suggestion-search" name="q" placeholder="Search IMDb" aria-label="Search IMDb" autoCapitalize="off" autoCorrect="off" spellcheck="true"/></div></div><button id="suggestion-search-button" type="submit" aria-label="Submit Search" class="nav-search__search-submit searchform__submit"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--magnify" id="iconContext-magnify" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M15.5 14h-.79l-.28-.27a6.5 6.5 0 0 0 1.48-5.34c-.47-2.78-2.79-5-5.59-5.34a6.505 6.505 0 0 0-7.27 7.27c.34 2.8 2.56 5.12 5.34 5.59a6.5 6.5 0 0 0 5.34-1.48l.27.28v.79l4.25 4.25c.41.41 1.08.41 1.49 0 .41-.41.41-1.08 0-1.49L15.5 14zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"></path></svg></button><input type="hidden" name="ref_" value="nv_sr_sm"/></form><label id="imdbHeader-searchClose" class="ipc-icon-button imdb-header-search__state-closer ipc-icon-button--baseAlt ipc-icon-button--onBase" title="Close Search" role="button" tabindex="0" aria-label="Close Search" aria-disabled="false" for="navSearch-searchState"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--clear" id="iconContext-clear" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M18.3 5.71a.996.996 0 0 0-1.41 0L12 10.59 7.11 5.7A.996.996 0 1 0 5.7 7.11L10.59 12 5.7 16.89a.996.996 0 1 0 1.41 1.41L12 13.41l4.89 4.89a.996.996 0 1 0 1.41-1.41L13.41 12l4.89-4.89c.38-.38.38-1.02 0-1.4z"></path></svg></label></div><label id="imdbHeader-searchOpen" class="ipc-icon-button sc-kDDrLX cxVnln imdb-header-search__state-opener ipc-icon-button--baseAlt ipc-icon-button--onBase" title="Open Search" role="button" tabindex="0" aria-label="Open Search" aria-disabled="false" for="navSearch-searchState"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--magnify" id="iconContext-magnify" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M15.5 14h-.79l-.28-.27a6.5 6.5 0 0 0 1.48-5.34c-.47-2.78-2.79-5-5.59-5.34a6.505 6.505 0 0 0-7.27 7.27c.34 2.8 2.56 5.12 5.34 5.59a6.5 6.5 0 0 0 5.34-1.48l.27.28v.79l4.25 4.25c.41.41 1.08.41 1.49 0 .41-.41.41-1.08 0-1.49L15.5 14zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"></path></svg></label><div class="navbar__imdbpro sc-iIPllB ecLOfK"><div class="sc-iBkjds iosfhR navbar__imdbpro-content navbar__flyout--breakpoint-l"><a class="ipc-button ipc-button--single-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-baseAlt ipc-button--theme-baseAlt ipc-button--on-textPrimary ipc-text-button navbar__flyout__text-button-after-mobile navbar__imdb-pro--toggle" role="button" tabindex="0" aria-label="Go To IMDb Pro" aria-disabled="false" href="https://pro.imdb.com/login/ap?u=/login/lwa&amp;imdbPageAction=signUp&amp;rf=cons_nb_hm&amp;ref_=cons_nb_hm"><div class="ipc-button__text"><svg class="ipc-logo navbar__imdbpro-menu-toggle__name" width="52" height="14" viewBox="0 0 52 14" xmlns="http://www.w3.org/2000/svg" version="1.1"><g fill="currentColor"><rect x="0" y="1" width="3.21" height="12.34"></rect><path d="M10,1 L9.3,6.76 L8.84,3.63 C8.7,2.62 8.58,1.75 8.45,1 L4.3,1 L4.3,13.34 L7.11,13.34 L7.11,5.19 L8.3,13.34 L10.3,13.34 L11.42,5 L11.42,13.33 L14.22,13.33 L14.22,1 L10,1 Z"></path><path d="M19.24,3.22 C19.3711159,3.29185219 19.4602235,3.42180078 19.48,3.57 C19.5340993,3.92393477 19.554191,4.28223587 19.54,4.64 L19.54,9.42 C19.578852,9.92887392 19.5246327,10.4405682 19.38,10.93 C19.27,11.12 18.99,11.21 18.53,11.21 L18.53,3.11 C18.7718735,3.09406934 19.0142863,3.13162626 19.24,3.22 Z M19.24,13.34 C19.8163127,13.3574057 20.3928505,13.3138302 20.96,13.21 C21.3245396,13.1481159 21.6680909,12.9969533 21.96,12.77 C22.2288287,12.5438006 22.4209712,12.2398661 22.51,11.9 C22.643288,11.1679419 22.6969338,10.4236056 22.67,9.68 L22.67,5.34 C22.6662002,4.55669241 22.6060449,3.77467335 22.49,3 C22.43037,2.59841431 22.260779,2.22116094 22,1.91 C21.6636187,1.56093667 21.2326608,1.317654 20.76,1.21 C19.7709421,1.02848785 18.7647002,0.958050915 17.76,1 L15.32,1 L15.32,13.34 L19.24,13.34 Z"></path><path d="M27.86,10.34 C27.8769902,10.7218086 27.8501483,11.1043064 27.78,11.48 C27.72,11.63 27.46,11.71 27.26,11.71 C27.0954951,11.7299271 26.9386363,11.6349863 26.88,11.48 C26.7930212,11.1542289 26.7592527,10.8165437 26.78,10.48 L26.78,7.18 C26.7626076,6.84408875 26.7929089,6.50740774 26.87,6.18 C26.9317534,6.03447231 27.0833938,5.94840616 27.24,5.97 C27.43,5.97 27.7,6.05 27.76,6.21 C27.8468064,6.53580251 27.8805721,6.87345964 27.86,7.21 L27.86,10.34 Z M23.7,1 L23.7,13.34 L26.58,13.34 L26.78,12.55 C27.0112432,12.8467609 27.3048209,13.0891332 27.64,13.26 C28.0022345,13.4198442 28.394069,13.5016184 28.79,13.5 C29.2588971,13.515288 29.7196211,13.3746089 30.1,13.1 C30.4399329,12.8800058 30.6913549,12.5471372 30.81,12.16 C30.9423503,11.6167622 31.0061799,11.0590937 31,10.5 L31,7 C31.0087531,6.51279482 30.9920637,6.02546488 30.95,5.54 C30.904474,5.28996521 30.801805,5.05382649 30.65,4.85 C30.4742549,4.59691259 30.2270668,4.40194735 29.94,4.29 C29.5869438,4.15031408 29.2096076,4.08232558 28.83,4.09 C28.4361722,4.08961884 28.0458787,4.16428368 27.68,4.31 C27.3513666,4.46911893 27.0587137,4.693713 26.82,4.97 L26.82,1 L23.7,1 Z"></path></g><g fill="#33C5F3"><path d="M32.13,1 L35.32,1 C35.9925574,0.978531332 36.6650118,1.04577677 37.32,1.2 C37.717112,1.29759578 38.0801182,1.50157071 38.37,1.79 C38.6060895,2.05302496 38.7682605,2.37391646 38.84,2.72 C38.935586,3.27463823 38.9757837,3.8374068 38.96,4.4 L38.96,5.46 C38.9916226,6.03689533 38.9100917,6.61440551 38.72,7.16 C38.5402933,7.53432344 38.2260614,7.82713037 37.84,7.98 C37.3049997,8.18709035 36.7332458,8.28238268 36.16,8.26 L35.31,8.26 L35.31,13.16 L32.13,13.16 L32.13,1 Z M35.29,3.08 L35.29,6.18 L35.53,6.18 C35.7515781,6.20532753 35.9725786,6.12797738 36.13,5.97 C36.2717869,5.69610033 36.3308522,5.38687568 36.3,5.08 L36.3,4.08 C36.3390022,3.79579475 36.2713114,3.5072181 36.11,3.27 C35.8671804,3.11299554 35.5771259,3.04578777 35.29,3.08 Z"></path><path d="M42,4.36 L41.89,5.52 C42.28,4.69 43.67,4.42 44.41,4.37 L43.6,7.3 C43.2290559,7.27725357 42.8582004,7.34593052 42.52,7.5 C42.3057075,7.61238438 42.1519927,7.81367763 42.1,8.05 C42.0178205,8.59259006 41.9843538,9.14144496 42,9.69 L42,13.16 L39.34,13.16 L39.34,4.36 L42,4.36 Z"></path><path d="M51.63,9.71 C51.6472876,10.3265292 51.6003682,10.9431837 51.49,11.55 C51.376862,11.9620426 51.1639158,12.3398504 50.87,12.65 C50.5352227,13.001529 50.1148049,13.2599826 49.65,13.4 C49.0994264,13.5686585 48.5257464,13.6496486 47.95,13.64 C47.3333389,13.6524659 46.7178074,13.5818311 46.12,13.43 C45.6996896,13.322764 45.3140099,13.1092627 45,12.81 C44.7275808,12.5275876 44.5254637,12.1850161 44.41,11.81 C44.2627681,11.2181509 44.1921903,10.6098373 44.2,10 L44.2,7.64 C44.1691064,6.9584837 44.2780071,6.27785447 44.52,5.64 C44.7547114,5.12751365 45.1616363,4.71351186 45.67,4.47 C46.3337168,4.13941646 47.0688388,3.97796445 47.81,4 C48.4454888,3.98667568 49.0783958,4.08482705 49.68,4.29 C50.1352004,4.42444561 50.5506052,4.66819552 50.89,5 C51.1535526,5.26601188 51.3550281,5.58700663 51.48,5.94 C51.6001358,6.42708696 51.6506379,6.92874119 51.63,7.43 L51.63,9.71 Z M48.39,6.73 C48.412199,6.42705368 48.3817488,6.12255154 48.3,5.83 C48.2091142,5.71223121 48.0687606,5.64325757 47.92,5.64325757 C47.7712394,5.64325757 47.6308858,5.71223121 47.54,5.83 C47.447616,6.12046452 47.4136298,6.42634058 47.44,6.73 L47.44,10.93 C47.4168299,11.2204468 47.4508034,11.5126191 47.54,11.79 C47.609766,11.9270995 47.7570827,12.0067302 47.91,11.99 C48.0639216,12.0108082 48.2159732,11.9406305 48.3,11.81 C48.3790864,11.5546009 48.4096133,11.2866434 48.39,11.02 L48.39,6.73 Z"></path></g></svg></div></a></div></div><div class="sc-cCsOjp efUGki"></div><div class="sc-llJcti lkeUvO imdb-header__watchlist-button"><a class="ipc-button ipc-button--single-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-baseAlt ipc-button--theme-baseAlt ipc-button--on-textPrimary ipc-text-button" role="button" tabindex="0" aria-disabled="false" href="/list/watchlist?ref_=nv_usr_wl_all_0"><svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--watchlist ipc-button__icon ipc-button__icon--pre" id="iconContext-watchlist" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M17 3c1.05 0 1.918.82 1.994 1.851L19 5v16l-7-3-7 3V5c0-1.05.82-1.918 1.851-1.994L7 3h10zm-4 4h-2v3H8v2h3v3h2v-3h3v-2h-3V7z" fill="currentColor"></path></svg><div class="ipc-button__text">Watchlist</div></a></div><div class="nav__userMenu navbar__user sc-cxabCf eIxWAw"><a class="ipc-button ipc-button--single-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-baseAlt ipc-button--theme-baseAlt ipc-button--on-textPrimary ipc-text-button imdb-header__signin-text" role="button" tabindex="0" aria-disabled="false" href="/registration/signin?ref=nv_generic_lgin&amp;u="><div class="ipc-button__text">Sign In</div></a></div><div class="sc-iBkjds sc-ikZpkk iosfhR cWEknO navbar__flyout--breakpoint-m"><label class="ipc-button ipc-button--single-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-baseAlt ipc-button--theme-baseAlt ipc-button--on-textDisabled ipc-text-button navbar__flyout__text-button-after-mobile" role="button" tabindex="0" aria-label="Toggle language selector" aria-disabled="false" for="nav-language-selector"><div class="ipc-button__text">EN<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--arrow-drop-down navbar__flyout__button-pointer" id="iconContext-arrow-drop-down" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M8.71 11.71l2.59 2.59c.39.39 1.02.39 1.41 0l2.59-2.59c.63-.63.18-1.71-.71-1.71H9.41c-.89 0-1.33 1.08-.7 1.71z"></path></svg></div></label><input type="checkbox" class="ipc-menu__focused-state" id="nav-language-selector" name="nav-language-selector" hidden="" tabindex="-1" aria-hidden="true"/><div class="ipc-menu mdc-menu ipc-menu--not-initialized ipc-menu--on-baseAlt ipc-menu--anchored ipc-menu--with-checkbox ipc-menu--expand-from-top-right navbar__flyout--menu" data-menu-id="nav-language-selector" role="presentation"><div class="ipc-menu__items mdc-menu__items" role="presentation"><span id="nav-language-selector-contents"><ul class="ipc-list nav-fully-supported-languages ipc-list--baseAlt" role="menu" aria-orientation="vertical"><li role="menuitem" class="ipc-list__item sc-jIZahH bIXmnE disabled" tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Fully supported</span></li><li role="separator" class="ipc-list-divider"></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled selected" id="language-option-en-US" aria-label="English (United States)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-checked language-menu-item-icon selected-language-icon" id="iconContext-radio-button-checked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path><circle cx="12" cy="12" r="5"></circle></svg></span><span class="ipc-list-item__text" role="presentation">English (United States)</span></li><li role="separator" class="ipc-list-divider"></li></ul><ul class="ipc-list nav-partially-supported-languages ipc-list--baseAlt" role="menu" aria-orientation="vertical"><a role="menuitem" class="ipc-list__item sc-jIZahH bIXmnE" href="https://help.imdb.com/article/issues/G6TCMBKAAR5G95EN?ref_=loc_nav_sel_hlp" target="_blank" aria-label="Learn more about partially supported languages." tabindex="0" aria-disabled="false"><span class="ipc-list-item__text" role="presentation">Partially supported</span><span class="ipc-list-item__icon ipc-list-item__icon--post" role="presentation"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--help-outline" id="iconContext-help-outline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm-1-4h2v2h-2zm1.61-9.96c-2.06-.3-3.88.97-4.43 2.79-.18.58.26 1.17.87 1.17h.2c.41 0 .74-.29.88-.67.32-.89 1.27-1.5 2.3-1.28.95.2 1.65 1.13 1.57 2.1-.1 1.34-1.62 1.63-2.45 2.88 0 .01-.01.01-.01.02-.01.02-.02.03-.03.05-.09.15-.18.32-.25.5-.01.03-.03.05-.04.08-.01.02-.01.04-.02.07-.12.34-.2.75-.2 1.25h2c0-.42.11-.77.28-1.07.02-.03.03-.06.05-.09.08-.14.18-.27.28-.39.01-.01.02-.03.03-.04.1-.12.21-.23.33-.34.96-.91 2.26-1.65 1.99-3.56-.24-1.74-1.61-3.21-3.35-3.47z"></path></svg></span></a><li role="separator" class="ipc-list-divider"></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-fr-CA" aria-label="Fran\xc3\xa7ais (Canada)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">Fran\xc3\xa7ais (Canada)</span></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-fr-FR" aria-label="Fran\xc3\xa7ais (France)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">Fran\xc3\xa7ais (France)</span></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-de-DE" aria-label="Deutsch (Deutschland)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">Deutsch (Deutschland)</span></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-hi-IN" aria-label="\xe0\xa4\xb9\xe0\xa4\xbf\xe0\xa4\x82\xe0\xa4\xa6\xe0\xa5\x80 (\xe0\xa4\xad\xe0\xa4\xbe\xe0\xa4\xb0\xe0\xa4\xa4)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">\xe0\xa4\xb9\xe0\xa4\xbf\xe0\xa4\x82\xe0\xa4\xa6\xe0\xa5\x80 (\xe0\xa4\xad\xe0\xa4\xbe\xe0\xa4\xb0\xe0\xa4\xa4)</span></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-it-IT" aria-label="Italiano (Italia)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">Italiano (Italia)</span></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-pt-BR" aria-label="Portugu\xc3\xaas (Brasil)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">Portugu\xc3\xaas (Brasil)</span></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-es-ES" aria-label="Espa\xc3\xb1ol (Espa\xc3\xb1a)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">Espa\xc3\xb1ol (Espa\xc3\xb1a)</span></li><li role="menuitem" class="ipc-list__item sc-ezWOiH crObts language-option disabled" id="language-option-es-MX" aria-label="Espa\xc3\xb1ol (M\xc3\xa9xico)" tabindex="0" aria-disabled="false"><span role="presentation" class="language-menu-item-span ipc-list-item__icon ipc-list-item__icon--pre"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" class="ipc-icon ipc-icon--radio-button-unchecked language-menu-item-icon " id="iconContext-radio-button-unchecked" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path fill="none" d="M0 0h24v24H0V0z"></path><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"></path></svg></span><span class="ipc-list-item__text" role="presentation">Espa\xc3\xb1ol (M\xc3\xa9xico)</span></li></ul></span></div></div></div></div></nav><script>if(typeof uet === \'function\'){ uet(\'be\', \'imdbHeader\', {wb: 1}); }</script><script>if(typeof uex === \'function\'){ uex(\'ld\', \'imdbHeader\', {wb: 1}); }</script><svg style="width:0;height:0;overflow:hidden;display:block" xmlns="http://www.w3.org/2000/svg" version="1.1"><defs><linearGradient id="ipc-svg-gradient-tv-logo-t" x1="31.973%" y1="53.409%" x2="153.413%" y2="-16.853%"><stop stop-color="#D01F49" offset="21.89%"></stop><stop stop-color="#E8138B" offset="83.44%"></stop></linearGradient><linearGradient id="ipc-svg-gradient-tv-logo-v" x1="-38.521%" y1="84.997%" x2="104.155%" y2="14.735%"><stop stop-color="#D01F49" offset="21.89%"></stop><stop stop-color="#E8138B" offset="83.44%"></stop></linearGradient></defs></svg></section>        <script>\n            window.__NAVBAR_PROPS__ = {\n                locale: \'en-US\',\n                isLoggedIn: false,\n                serviceName: \'Legacy\',\n                showLanguageSelector: true,\n                coachmarkWeblabTreatment: \'C\'\n            };\n        </script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("ne");\n    }\n</script>\n\n        <div id="wrapper">\n            <div id="root" class="redesign">\n                <div id="nb20" class="navbarSprite">\n                    <div id="supertab">\t\n        <!-- no content received for slot: top_ad -->\n            <script>\n                var event = {\n                    type: \'\',\n                    slotName: \'top_ad\',\n                    timestamp: Date.now()\n                };\n                var mediaEvent = event;\n                mediaEvent.type = \'no-autoplay-video-ad-detected\';\n                if (window && window.mediaOrchestrator) {\n                    window.mediaOrchestrator.publish(\'mediaPlaybackEvent\', mediaEvent);\n                    window.mediaOrchestrator.publish(\'noAdToLoad\', event);\n                }\n            </script>\n\t\n</div>\n\t\n        <!-- no content received for slot: navstrip -->\n\t\n\t\n        <!-- no content received for slot: injected_navstrip -->\n\t\n                </div>\n              \n\n               <div id="pagecontent" class="pagecontent">\n\t\n        <!-- no content received for slot: injected_billboard -->\n\t\n<div id="content-2-wide" class="redesign">\n    <div id="main">\n        <div class="article">\n            <h1 class="header">Feature Film,\nReleased between 2018-01-01 and 2020-12-31\n(Sorted by Number of Votes Descending) </h1>\n            <div class="nav">\n                <br class="clear" />\n    <div class="display-mode float-right">\n        View Mode:\n<a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=num_votes,desc&view=simple"\nclass="compact" > Compact\n</a>    <span class="ghost">|</span><a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=num_votes,desc&view=advanced"\nclass="detailed" > <strong>Detailed</strong>\n</a>    </div>\n    <div class="desc">\n        <span>1-50 of 35,913 titles.</span>\n    <span class="ghost">|</span>        <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=num_votes,desc&start=51"\nclass="lister-page-next next-page" >Next &#187;</a>\n    </div>\n            </div>\n   <br class="clear" />\n   <div class="sorting">\n       Sort by:\n           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31"\n>Popularity</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=alpha,asc"\n>A-Z</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=user_rating,desc"\n>User Rating</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=num_votes,asc"\n><strong>Number of Votes</strong>&#x25BC;</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=boxoffice_gross_us,asc"\n>US Box Office</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=runtime,asc"\n>Runtime</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=year,asc"\n>Year</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=release_date,asc"\n>Release Date</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=your_rating_date,asc"\n>Date of Your Rating</a>\n    <span class="ghost">|</span>           <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=my_ratings,desc"\n>Your Rating</a>\n   </div>\n\n    <br class="clear" />\n    <div class="lister list detail sub-list">\n        <div class="lister-list">\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7286456" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7286456/"\n> <img alt="Joker"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BNGVjNWI4ZGUtNzE0MS00YTJmLWE0ZDctN2ZiYTk2YmI3NTYyXkEyXkFqcGdeQXVyMTkxNjUyNQ@@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7286456"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">1.</span>\n    \n    <a href="/title/tt7286456/"\n>Joker</a>\n    <span class="lister-item-year text-muted unbold">(I) (2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">122 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nCrime, Drama, Thriller            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7286456" data-tconst="tt7286456">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7286456">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7286456|imdb|8.4|8.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.4/10 (1,271,160 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="1271160" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 117.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7286456/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">59        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA mentally troubled stand-up comedian embarks on a downward spiral that leads to the creation of an iconic villain.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0680846/"\n>Todd Phillips</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0001618/"\n>Joaquin Phoenix</a>, \n<a href="/name/nm0000134/"\n>Robert De Niro</a>, \n<a href="/name/nm5939164/"\n>Zazie Beetz</a>, \n<a href="/name/nm0175814/"\n>Frances Conroy</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="1271160">1,271,160</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="335,451,311">$335.45M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt4154796" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt4154796/"\n> <img alt="Avengers: Endgame"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTc5MDE2ODcwNV5BMl5BanBnXkFtZTgwMzI2NzQ2NzM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt4154796"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">2.</span>\n    \n    <a href="/title/tt4154796/"\n>Avengers: Endgame</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">181 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt4154796" data-tconst="tt4154796">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt4154796">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt4154796|imdb|8.4|8.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.4/10 (1,121,441 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="1121441" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 117.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt4154796/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">78        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nAfter the devastating events of <a href="/title/tt4154756">Avengers: Infinity War</a> (2018), the universe is in ruins. With the help of remaining allies, the Avengers assemble once more in order to reverse Thanos\' actions and restore balance to the universe.</p>\n    <p class="">\n    Directors:\n<a href="/name/nm0751577/"\n>Anthony Russo</a>, \n<a href="/name/nm0751648/"\n>Joe Russo</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000375/"\n>Robert Downey Jr.</a>, \n<a href="/name/nm0262635/"\n>Chris Evans</a>, \n<a href="/name/nm0749263/"\n>Mark Ruffalo</a>, \n<a href="/name/nm1165110/"\n>Chris Hemsworth</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="1121441">1,121,441</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="858,373,000">$858.37M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt4154756" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt4154756/"\n> <img alt="Avengers: Infinity War"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMjMxNjY2MDU1OV5BMl5BanBnXkFtZTgwNzY1MTUwNTM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt4154756"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">3.</span>\n    \n    <a href="/title/tt4154756/"\n>Avengers: Infinity War</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">149 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt4154756" data-tconst="tt4154756">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt4154756">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt4154756|imdb|8.4|8.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.4/10 (1,073,310 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="1073310" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 117.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt4154756/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">68        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nThe Avengers and their allies must be willing to sacrifice all in an attempt to defeat the powerful Thanos before his blitz of devastation and ruin puts an end to the universe.</p>\n    <p class="">\n    Directors:\n<a href="/name/nm0751577/"\n>Anthony Russo</a>, \n<a href="/name/nm0751648/"\n>Joe Russo</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000375/"\n>Robert Downey Jr.</a>, \n<a href="/name/nm1165110/"\n>Chris Hemsworth</a>, \n<a href="/name/nm0749263/"\n>Mark Ruffalo</a>, \n<a href="/name/nm0262635/"\n>Chris Evans</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="1073310">1,073,310</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="678,815,482">$678.82M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6751668" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6751668/"\n> <img alt="Parasite"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BYWZjMjk3ZTItODQ2ZC00NTY5LWE0ZDYtZTI3MjcwN2Q5NTVkXkEyXkFqcGdeQXVyODk4OTc3MTY@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6751668"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">4.</span>\n    \n    <a href="/title/tt6751668/"\n>Parasite</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">132 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nDrama, Thriller            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.5">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.5</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6751668" data-tconst="tt6751668">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6751668">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6751668|imdb|8.5|8.5|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.5/10 (794,402 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.5" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="794402" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 119px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.5</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6751668/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">96        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nGreed and class discrimination threaten the newly formed symbiotic relationship between the wealthy Park family and the destitute Kim clan.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0094435/"\n>Bong Joon Ho</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0814280/"\n>Song Kang-ho</a>, \n<a href="/name/nm1310525/"\n>Lee Sun-kyun</a>, \n<a href="/name/nm1856097/"\n>Cho Yeo-jeong</a>, \n<a href="/name/nm6079248/"\n>Choi Woo-sik</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="794402">794,402</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="53,367,844">$53.37M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1825683" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1825683/"\n> <img alt="Black Panther"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTg1MTY2MjYzNV5BMl5BanBnXkFtZTgwMTc4NTMwNDI@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1825683"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">5.</span>\n    \n    <a href="/title/tt1825683/"\n>Black Panther</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">134 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.3">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.3</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1825683" data-tconst="tt1825683">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1825683">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1825683|imdb|7.3|7.3|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.3/10 (767,078 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.3" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="767078" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 102.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.3</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1825683/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">88        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nT\'Challa, heir to the hidden but advanced kingdom of Wakanda, must step forward to lead his people into a new future and must confront a challenger from his country\'s past.</p>\n    <p class="">\n    Director:\n<a href="/name/nm3363032/"\n>Ryan Coogler</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm1569276/"\n>Chadwick Boseman</a>, \n<a href="/name/nm0430107/"\n>Michael B. Jordan</a>, \n<a href="/name/nm2143282/"\n>Lupita Nyong\'o</a>, \n<a href="/name/nm1775091/"\n>Danai Gurira</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="767078">767,078</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="700,059,566">$700.06M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7131622" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7131622/"\n> <img alt="Once Upon a Time in Hollywood"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BOTg4ZTNkZmUtMzNlZi00YmFjLTk1MmUtNWQwNTM0YjcyNTNkXkEyXkFqcGdeQXVyNjg2NjQwMDQ@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7131622"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">6.</span>\n    \n    <a href="/title/tt7131622/"\n>Once Upon a Time in Hollywood</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">161 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nComedy, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.6">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.6</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7131622" data-tconst="tt7131622">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7131622">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7131622|imdb|7.6|7.6|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.6/10 (731,836 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.6" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="731836" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 106.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.6</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7131622/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">83        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA faded television actor and his stunt double strive to achieve fame and success in the final years of Hollywood\'s Golden Age in 1969 Los Angeles.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0000233/"\n>Quentin Tarantino</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000138/"\n>Leonardo DiCaprio</a>, \n<a href="/name/nm0000093/"\n>Brad Pitt</a>, \n<a href="/name/nm3053338/"\n>Margot Robbie</a>, \n<a href="/name/nm0386472/"\n>Emile Hirsch</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="731836">731,836</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="142,502,728">$142.50M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt8946378" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt8946378/"\n> <img alt="Knives Out"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMGUwZjliMTAtNzAxZi00MWNiLWE2NzgtZGUxMGQxZjhhNDRiXkEyXkFqcGdeQXVyNjU1NzU3MzE@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt8946378"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">7.</span>\n    \n    <a href="/title/tt8946378/"\n>Knives Out</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">130 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nComedy, Crime, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.9">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.9</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt8946378" data-tconst="tt8946378">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt8946378">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt8946378|imdb|7.9|7.9|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.9/10 (625,518 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.9" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="625518" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 110.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.9</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt8946378/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">82        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA detective investigates the death of the patriarch of an eccentric, combative family.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0426059/"\n>Rian Johnson</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0185819/"\n>Daniel Craig</a>, \n<a href="/name/nm0262635/"\n>Chris Evans</a>, \n<a href="/name/nm1869101/"\n>Ana de Armas</a>, \n<a href="/name/nm0000130/"\n>Jamie Lee Curtis</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="625518">625,518</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="165,359,751">$165.36M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt8579674" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt8579674/"\n> <img alt="1917"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BOTdmNTFjNDEtNzg0My00ZjkxLTg1ZDAtZTdkMDc2ZmFiNWQ1XkEyXkFqcGdeQXVyNTAzNzgwNTg@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt8579674"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">8.</span>\n    \n    <a href="/title/tt8579674/"\n>1917</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">119 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Drama, War            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.2">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.2</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt8579674" data-tconst="tt8579674">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt8579674">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt8579674|imdb|8.2|8.2|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.2/10 (585,333 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.2" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="585333" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 114.8px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.2</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt8579674/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">78        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nApril 6th, 1917. As an infantry battalion assembles to wage war deep in enemy territory, two soldiers are assigned to race against time and deliver a message that will stop 1,600 men from walking straight into a deadly trap.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0005222/"\n>Sam Mendes</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm2835616/"\n>Dean-Charles Chapman</a>, \n<a href="/name/nm1126657/"\n>George MacKay</a>, \n<a href="/name/nm0990547/"\n>Daniel Mays</a>, \n<a href="/name/nm0000147/"\n>Colin Firth</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="585333">585,333</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="159,227,644">$159.23M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt5463162" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt5463162/"\n> <img alt="Deadpool 2"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMDkzNmRhNTMtZDI4NC00Zjg1LTgxM2QtMjYxZDQ3OWJlMDRlXkEyXkFqcGdeQXVyNTU5MjkzMTU@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt5463162"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">9.</span>\n    \n    <a href="/title/tt5463162/"\n>Deadpool 2</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">119 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Comedy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.7">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.7</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt5463162" data-tconst="tt5463162">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt5463162">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt5463162|imdb|7.7|7.7|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.7/10 (581,103 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.7" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="581103" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 107.8px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.7</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt5463162/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">66        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nFoul-mouthed mutant mercenary Wade Wilson (a.k.a. Deadpool) assembles a team of fellow mutant rogues to protect a young boy with supernatural abilities from the brutal, time-traveling cyborg Cable.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0500610/"\n>David Leitch</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0005351/"\n>Ryan Reynolds</a>, \n<a href="/name/nm0000982/"\n>Josh Brolin</a>, \n<a href="/name/nm1072555/"\n>Morena Baccarin</a>, \n<a href="/name/nm5421877/"\n>Julian Dennison</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="581103">581,103</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="324,591,735">$324.59M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt4154664" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt4154664/"\n> <img alt="Captain Marvel"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTE0YWFmOTMtYTU2ZS00ZTIxLWE3OTEtYTNiYzBkZjViZThiXkEyXkFqcGdeQXVyODMzMzQ4OTI@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt4154664"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">10.</span>\n    \n    <a href="/title/tt4154664/"\n>Captain Marvel</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">123 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.8</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt4154664" data-tconst="tt4154664">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt4154664">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt4154664|imdb|6.8|6.8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.8/10 (559,300 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="559300" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 95.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt4154664/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">64        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nCarol Danvers becomes one of the universe\'s most powerful heroes when Earth is caught in the middle of a galactic war between two alien races.</p>\n    <p class="">\n    Directors:\n<a href="/name/nm1349818/"\n>Anna Boden</a>, \n<a href="/name/nm0281396/"\n>Ryan Fleck</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0488953/"\n>Brie Larson</a>, \n<a href="/name/nm0000168/"\n>Samuel L. Jackson</a>, \n<a href="/name/nm0578853/"\n>Ben Mendelsohn</a>, \n<a href="/name/nm0000179/"\n>Jude Law</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="559300">559,300</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="426,829,839">$426.83M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1727824" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1727824/"\n> <img alt="Bohemian Rhapsody"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTA2NDc3Njg5NDVeQTJeQWpwZ15BbWU4MDc1NDcxNTUz._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1727824"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">11.</span>\n    \n    <a href="/title/tt1727824/"\n>Bohemian Rhapsody</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">134 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nBiography, Drama, Music            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.9">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.9</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1727824" data-tconst="tt1727824">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1727824">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1727824|imdb|7.9|7.9|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.9/10 (537,882 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.9" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="537882" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 110.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.9</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1727824/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">49        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nThe story of the legendary British rock band <a href="/name/nm1277102">Queen</a> and lead singer <a href="/name/nm0006198">Freddie Mercury</a>, leading up to their famous performance at <a href="/title/tt0261024">Live Aid</a> (1985).</p>\n    <p class="">\n    Director:\n<a href="/name/nm0001741/"\n>Bryan Singer</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm1785339/"\n>Rami Malek</a>, \n<a href="/name/nm2377903/"\n>Lucy Boynton</a>, \n<a href="/name/nm3152605/"\n>Gwilym Lee</a>, \n<a href="/name/nm5228887/"\n>Ben Hardy</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="537882">537,882</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="216,428,042">$216.43M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6644200" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6644200/"\n> <img alt="A Quiet Place"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMjI0MDMzNTQ0M15BMl5BanBnXkFtZTgwMTM5NzM3NDM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6644200"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">12.</span>\n    \n    <a href="/title/tt6644200/"\n>A Quiet Place</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">90 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nDrama, Horror, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.5">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.5</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6644200" data-tconst="tt6644200">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6644200">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6644200|imdb|7.5|7.5|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.5/10 (528,070 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.5" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="528070" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 105px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.5</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6644200/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">82        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nIn a post-apocalyptic world, a family is forced to live in silence while hiding from monsters with ultra-sensitive hearing.</p>\n    <p class="">\n    Director:\n<a href="/name/nm1024677/"\n>John Krasinski</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm1289434/"\n>Emily Blunt</a>, \n<a href="/name/nm1024677/"\n>John Krasinski</a>, \n<a href="/name/nm8075925/"\n>Millicent Simmonds</a>, \n<a href="/name/nm7415871/"\n>Noah Jupe</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="528070">528,070</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="188,024,361">$188.02M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt4633694" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt4633694/"\n> <img alt="Spider-Man: Into the Spider-Verse"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMjMwNDkxMTgzOF5BMl5BanBnXkFtZTgwNTkwNTQ3NjM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt4633694"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">13.</span>\n    \n    <a href="/title/tt4633694/"\n>Spider-Man: Into the Spider-Verse</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">117 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAnimation, Action, Adventure            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt4633694" data-tconst="tt4633694">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt4633694">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt4633694|imdb|8.4|8.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.4/10 (524,973 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="524973" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 117.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt4633694/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">87        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nTeen Miles Morales becomes the Spider-Man of his universe, and must join with five spider-powered individuals from other dimensions to stop a threat for all realities.</p>\n    <p class="">\n    Directors:\n<a href="/name/nm2130108/"\n>Bob Persichetti</a>, \n<a href="/name/nm0709056/"\n>Peter Ramsey</a>, \n<a href="/name/nm0745247/"\n>Rodney Rothman</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm4271336/"\n>Shameik Moore</a>, \n<a href="/name/nm2159926/"\n>Jake Johnson</a>, \n<a href="/name/nm2794962/"\n>Hailee Steinfeld</a>, \n<a href="/name/nm0991810/"\n>Mahershala Ali</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="524973">524,973</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="190,241,310">$190.24M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6723592" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6723592/"\n> <img alt="Tenet"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BYzg0NGM2NjAtNmIxOC00MDJmLTg5ZmYtYzM0MTE4NWE2NzlhXkEyXkFqcGdeQXVyMTA4NjE0NjEy._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6723592"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">14.</span>\n    \n    <a href="/title/tt6723592/"\n>Tenet</a>\n    <span class="lister-item-year text-muted unbold">(2020)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">150 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Sci-Fi, Thriller            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.3">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.3</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6723592" data-tconst="tt6723592">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6723592">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6723592|imdb|7.3|7.3|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.3/10 (501,133 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.3" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="501133" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 102.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.3</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6723592/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">69        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nArmed with only one word, Tenet, and fighting for the survival of the entire world, a Protagonist journeys through a twilight world of international espionage on a mission that will unfold in something beyond real time.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0634240/"\n>Christopher Nolan</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0913475/"\n>John David Washington</a>, \n<a href="/name/nm1500155/"\n>Robert Pattinson</a>, \n<a href="/name/nm4456120/"\n>Elizabeth Debicki</a>, \n<a href="/name/nm1798449/"\n>Juhan Ulfsak</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="501133">501,133</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="58,456,624">$58.46M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6320628" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6320628/"\n> <img alt="Spider-Man: Far from Home"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMGZlNTY1ZWUtYTMzNC00ZjUyLWE0MjQtMTMxN2E3ODYxMWVmXkEyXkFqcGdeQXVyMDM2NDM2MQ@@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6320628"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">15.</span>\n    \n    <a href="/title/tt6320628/"\n>Spider-Man: Far from Home</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">129 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6320628" data-tconst="tt6320628">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6320628">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6320628|imdb|7.4|7.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.4/10 (490,780 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="490780" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 103.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6320628/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">69        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nFollowing the events of <a href="/title/tt4154796">Avengers: Endgame</a> (2019), Spider-Man must step up to take on new threats in a world that has changed forever.</p>\n    <p class="">\n    Director:\n<a href="/name/nm1218281/"\n>Jon Watts</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm4043618/"\n>Tom Holland</a>, \n<a href="/name/nm0000168/"\n>Samuel L. Jackson</a>, \n<a href="/name/nm0350453/"\n>Jake Gyllenhaal</a>, \n<a href="/name/nm0000673/"\n>Marisa Tomei</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="490780">490,780</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="390,532,085">$390.53M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6966692" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6966692/"\n> <img alt="Green Book"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BYzIzYmJlYTYtNGNiYy00N2EwLTk4ZjItMGYyZTJiOTVkM2RlXkEyXkFqcGdeQXVyODY1NDk1NjE@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6966692"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">16.</span>\n    \n    <a href="/title/tt6966692/"\n>Green Book</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">130 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nBiography, Comedy, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.2">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.2</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6966692" data-tconst="tt6966692">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6966692">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6966692|imdb|8.2|8.2|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.2/10 (489,297 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.2" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="489297" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 114.8px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.2</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6966692/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">69        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA working-class Italian-American bouncer becomes the driver for an African-American classical pianist on a tour of venues through the 1960s American South.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0268380/"\n>Peter Farrelly</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0001557/"\n>Viggo Mortensen</a>, \n<a href="/name/nm0991810/"\n>Mahershala Ali</a>, \n<a href="/name/nm0004802/"\n>Linda Cardellini</a>, \n<a href="/name/nm1724319/"\n>Sebastian Maniscalco</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="489297">489,297</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="85,080,171">$85.08M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1270797" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1270797/"\n> <img alt="Venom"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BNTFkZjdjN2QtOGE5MS00ZTgzLTgxZjAtYzkyZWQ5MjEzYmZjXkEyXkFqcGdeQXVyMTM0NTUzNDIy._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1270797"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">17.</span>\n    \n    <a href="/title/tt1270797/"\n>Venom</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">112 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.6">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.6</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1270797" data-tconst="tt1270797">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1270797">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1270797|imdb|6.6|6.6|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.6/10 (484,798 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.6" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="484798" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 92.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.6</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1270797/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  unfavorable">35        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA failed reporter is bonded to an alien entity, one of many symbiotes who have invaded Earth. But the being takes a liking to Earth and decides to protect it.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0281508/"\n>Ruben Fleischer</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0362766/"\n>Tom Hardy</a>, \n<a href="/name/nm0931329/"\n>Michelle Williams</a>, \n<a href="/name/nm1981893/"\n>Riz Ahmed</a>, \n<a href="/name/nm2020278/"\n>Scott Haze</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="484798">484,798</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="213,515,506">$213.52M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1477834" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1477834/"\n> <img alt="Aquaman"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BOTk5ODg0OTU5M15BMl5BanBnXkFtZTgwMDQ3MDY3NjM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1477834"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">18.</span>\n    \n    <a href="/title/tt1477834/"\n>Aquaman</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">143 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Fantasy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.8</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1477834" data-tconst="tt1477834">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1477834">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1477834|imdb|6.8|6.8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.8/10 (474,100 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="474100" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 95.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1477834/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">55        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nArthur Curry, the human-born heir to the underwater kingdom of Atlantis, goes on a quest to prevent a war between the worlds of ocean and land.</p>\n    <p class="">\n    Director:\n<a href="/name/nm1490123/"\n>James Wan</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0597388/"\n>Jason Momoa</a>, \n<a href="/name/nm1720028/"\n>Amber Heard</a>, \n<a href="/name/nm0000353/"\n>Willem Dafoe</a>, \n<a href="/name/nm0933940/"\n>Patrick Wilson</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="474100">474,100</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="335,061,807">$335.06M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt2527338" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt2527338/"\n> <img alt="Star Wars: The Rise Of Skywalker"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMDljNTQ5ODItZmQwMy00M2ExLTljOTQtZTVjNGE2NTg0NGIxXkEyXkFqcGdeQXVyODkzNTgxMDg@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt2527338"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">19.</span>\n    \n    <a href="/title/tt2527338/"\n>Star Wars: The Rise Of Skywalker</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">141 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Fantasy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.5">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.5</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt2527338" data-tconst="tt2527338">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt2527338">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt2527338|imdb|6.5|6.5|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.5/10 (450,323 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.5" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="450323" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 91px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.5</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt2527338/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">53        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nIn the riveting conclusion of the landmark Skywalker saga, new legends will be born-and the final battle for freedom is yet to come.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0009190/"\n>J.J. Abrams</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm5397459/"\n>Daisy Ridley</a>, \n<a href="/name/nm3915784/"\n>John Boyega</a>, \n<a href="/name/nm1209966/"\n>Oscar Isaac</a>, \n<a href="/name/nm3485845/"\n>Adam Driver</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="450323">450,323</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="515,202,542">$515.20M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1677720" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1677720/"\n> <img alt="Ready Player One"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BY2JiYTNmZTctYTQ1OC00YjU4LWEwMjYtZjkwY2Y5MDI0OTU3XkEyXkFqcGdeQXVyNTI4MzE4MDU@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1677720"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">20.</span>\n    \n    <a href="/title/tt1677720/"\n>Ready Player One</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">140 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1677720" data-tconst="tt1677720">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1677720">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1677720|imdb|7.4|7.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.4/10 (436,273 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="436273" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 103.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1677720/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">64        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nWhen the creator of a virtual reality called the OASIS dies, he makes a posthumous challenge to all OASIS users to find his Easter Egg, which will give the finder his fortune and control of his world.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0000229/"\n>Steven Spielberg</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm4446467/"\n>Tye Sheridan</a>, \n<a href="/name/nm4972453/"\n>Olivia Cooke</a>, \n<a href="/name/nm0578853/"\n>Ben Mendelsohn</a>, \n<a href="/name/nm2913119/"\n>Lena Waithe</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="436273">436,273</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="137,690,172">$137.69M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt5095030" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt5095030/"\n> <img alt="Ant-Man and the Wasp"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BYjcyYTk0N2YtMzc4ZC00Y2E0LWFkNDgtNjE1MzZmMGE1YjY1XkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt5095030"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">21.</span>\n    \n    <a href="/title/tt5095030/"\n>Ant-Man and the Wasp</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">118 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Comedy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.0</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt5095030" data-tconst="tt5095030">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt5095030">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt5095030|imdb|7|7|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7/10 (401,929 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="401929" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 98px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt5095030/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">70        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nAs Scott Lang balances being both a superhero and a father, Hope van Dyne and Dr. Hank Pym present an urgent new mission that finds the Ant-Man fighting alongside The Wasp to uncover secrets from their past.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0715636/"\n>Peyton Reed</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0748620/"\n>Paul Rudd</a>, \n<a href="/name/nm1431940/"\n>Evangeline Lilly</a>, \n<a href="/name/nm0671567/"\n>Michael Pe\xc3\xb1a</a>, \n<a href="/name/nm0324658/"\n>Walton Goggins</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="401929">401,929</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="216,648,740">$216.65M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1950186" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1950186/"\n> <img alt="Ford v Ferrari"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BM2UwMDVmMDItM2I2Yi00NGZmLTk4ZTUtY2JjNTQ3OGQ5ZjM2XkEyXkFqcGdeQXVyMTA1OTYzOTUx._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1950186"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">22.</span>\n    \n    <a href="/title/tt1950186/"\n>Ford v Ferrari</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">152 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Biography, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8.1">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.1</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1950186" data-tconst="tt1950186">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1950186">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1950186|imdb|8.1|8.1|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8.1/10 (392,038 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8.1" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="392038" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 113.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8.1</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1950186/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">81        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nAmerican car designer <a href="/name/nm0790961">Carroll Shelby</a> and driver <a href="/name/nm10752160">Ken Miles</a> battle corporate interference and the laws of physics to build a revolutionary race car for Ford in order to defeat Ferrari at the 24 Hours of Le Mans in 1966.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0003506/"\n>James Mangold</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000354/"\n>Matt Damon</a>, \n<a href="/name/nm0000288/"\n>Christian Bale</a>, \n<a href="/name/nm1256532/"\n>Jon Bernthal</a>, \n<a href="/name/nm1495520/"\n>Caitr\xc3\xadona Balfe</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="392038">392,038</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="117,624,028">$117.62M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt2584384" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt2584384/"\n> <img alt="Jojo Rabbit"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BZjU0Yzk2MzEtMjAzYy00MzY0LTg2YmItM2RkNzdkY2ZhN2JkXkEyXkFqcGdeQXVyNDg4NjY5OTQ@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt2584384"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">23.</span>\n    \n    <a href="/title/tt2584384/"\n>Jojo Rabbit</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">108 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nComedy, Drama, War            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.9">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.9</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt2584384" data-tconst="tt2584384">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt2584384">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt2584384|imdb|7.9|7.9|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.9/10 (388,250 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.9" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="388250" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 110.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.9</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt2584384/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">58        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA young German boy in the Hitler Youth whose hero and imaginary friend is the country\'s dictator is shocked to discover that his mother is hiding a Jewish girl in their home.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0169806/"\n>Taika Waititi</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm9877392/"\n>Roman Griffin Davis</a>, \n<a href="/name/nm5057169/"\n>Thomasin McKenzie</a>, \n<a href="/name/nm0424060/"\n>Scarlett Johansson</a>, \n<a href="/name/nm0169806/"\n>Taika Waititi</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="388250">388,250</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="33,370,906">$33.37M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1302006" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1302006/"\n> <img alt="The Irishman"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMGUyM2ZiZmUtMWY0OC00NTQ4LThkOGUtNjY2NjkzMDJiMWMwXkEyXkFqcGdeQXVyMzY0MTE3NzU@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1302006"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">24.</span>\n    \n    <a href="/title/tt1302006/"\n>The Irishman</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">209 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nBiography, Crime, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.8</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1302006" data-tconst="tt1302006">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1302006">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1302006|imdb|7.8|7.8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.8/10 (387,445 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="387445" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 109.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1302006/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">94        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nHitman Frank Sheeran looks back at the secrets he kept as a loyal member of the Bufalino crime family.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0000217/"\n>Martin Scorsese</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000134/"\n>Robert De Niro</a>, \n<a href="/name/nm0000199/"\n>Al Pacino</a>, \n<a href="/name/nm0000582/"\n>Joe Pesci</a>, \n<a href="/name/nm0000172/"\n>Harvey Keitel</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="387445">387,445</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="7,000,000">$7.00M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1517451" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1517451/"\n> <img alt="A Star Is Born"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BNmE5ZmE3OGItNTdlNC00YmMxLWEzNjctYzAwOGQ5ODg0OTI0XkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1517451"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">25.</span>\n    \n    <a href="/title/tt1517451/"\n>A Star Is Born</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">136 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nDrama, Music, Romance            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.6">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.6</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1517451" data-tconst="tt1517451">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1517451">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1517451|imdb|7.6|7.6|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.6/10 (384,163 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.6" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="384163" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 106.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.6</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1517451/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">88        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA musician helps a young singer find fame as age and alcoholism send his own career into a downward spiral.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0177896/"\n>Bradley Cooper</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm3078932/"\n>Lady Gaga</a>, \n<a href="/name/nm0177896/"\n>Bradley Cooper</a>, \n<a href="/name/nm0000385/"\n>Sam Elliott</a>, \n<a href="/name/nm0342399/"\n>Greg Grunberg</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="384163">384,163</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="215,288,866">$215.29M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt3778644" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt3778644/"\n> <img alt="Solo: A Star Wars Story"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BOTM2NTI3NTc3Nl5BMl5BanBnXkFtZTgwNzM1OTQyNTM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt3778644"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">26.</span>\n    \n    <a href="/title/tt3778644/"\n>Solo: A Star Wars Story</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">135 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.9">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.9</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt3778644" data-tconst="tt3778644">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt3778644">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt3778644|imdb|6.9|6.9|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.9/10 (347,373 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.9" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="347373" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 96.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.9</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt3778644/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">62        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nBoard the Millennium Falcon and journey to a galaxy far, far away in an epic action-adventure that will set the course of one of the Star Wars saga\'s most unlikely heroes.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0000165/"\n>Ron Howard</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm2403277/"\n>Alden Ehrenreich</a>, \n<a href="/name/nm0000437/"\n>Woody Harrelson</a>, \n<a href="/name/nm3592338/"\n>Emilia Clarke</a>, \n<a href="/name/nm2255973/"\n>Donald Glover</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="347373">347,373</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="213,767,512">$213.77M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6146586" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6146586/"\n> <img alt="John Wick: Chapter 3 - Parabellum"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMDg2YzI0ODctYjliMy00NTU0LTkxODYtYTNkNjQwMzVmOTcxXkEyXkFqcGdeQXVyNjg2NjQwMDQ@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6146586"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">27.</span>\n    \n    <a href="/title/tt6146586/"\n>John Wick: Chapter 3 - Parabellum</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">130 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Crime, Thriller            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6146586" data-tconst="tt6146586">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6146586">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6146586|imdb|7.4|7.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.4/10 (343,716 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="343716" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 103.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6146586/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">73        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nJohn Wick is on the run after killing a member of the international assassins\' guild, and with a $14 million price tag on his head, he is the target of hit men and women everywhere.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0821432/"\n>Chad Stahelski</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000206/"\n>Keanu Reeves</a>, \n<a href="/name/nm0000932/"\n>Halle Berry</a>, \n<a href="/name/nm0574534/"\n>Ian McShane</a>, \n<a href="/name/nm0000401/"\n>Laurence Fishburne</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="343716">343,716</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="171,015,687">$171.02M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt2737304" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt2737304/"\n> <img alt="Bird Box"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMjAzMTI1MjMyN15BMl5BanBnXkFtZTgwNzU5MTE2NjM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt2737304"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">28.</span>\n    \n    <a href="/title/tt2737304/"\n>Bird Box</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">124 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nHorror, Mystery, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.6">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.6</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt2737304" data-tconst="tt2737304">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt2737304">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt2737304|imdb|6.6|6.6|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.6/10 (342,267 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.6" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="342267" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 92.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.6</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt2737304/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">51        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nFive years after an ominous unseen presence drives most of society to suicide, a mother and her two children make a desperate bid to reach safety.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0081540/"\n>Susanne Bier</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000113/"\n>Sandra Bullock</a>, \n<a href="/name/nm5218990/"\n>Trevante Rhodes</a>, \n<a href="/name/nm0000518/"\n>John Malkovich</a>, \n<a href="/name/nm0005299/"\n>Sarah Paulson</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="342267">342,267</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt8367814" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt8367814/"\n> <img alt="The Gentlemen"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTlkMmVmYjktYTc2NC00ZGZjLWEyOWUtMjc2MDMwMjQwOTA5XkEyXkFqcGdeQXVyNTI4MzE4MDU@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt8367814"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">29.</span>\n    \n    <a href="/title/tt8367814/"\n>The Gentlemen</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">113 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Comedy, Crime            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.8</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt8367814" data-tconst="tt8367814">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt8367814">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt8367814|imdb|7.8|7.8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.8/10 (338,305 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="338305" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 109.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt8367814/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">51        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nAn American expat tries to sell off his highly profitable marijuana empire in London, triggering plots, schemes, bribery and blackmail in an attempt to steal his domain out from under him.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0005363/"\n>Guy Ritchie</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000190/"\n>Matthew McConaughey</a>, \n<a href="/name/nm0402271/"\n>Charlie Hunnam</a>, \n<a href="/name/nm1890784/"\n>Michelle Dockery</a>, \n<a href="/name/nm0834989/"\n>Jeremy Strong</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="338305">338,305</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="36,471,795">$36.47M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt0448115" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt0448115/"\n> <img alt="Shazam!"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BOWZhZjE4NGQtODg5Ni00MjQ1LWJmMzAtNzQ2N2M1NzYzMDJkXkEyXkFqcGdeQXVyMjMwNDgzNjc@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt0448115"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">30.</span>\n    \n    <a href="/title/tt0448115/"\n>Shazam!</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">132 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Comedy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.0</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt0448115" data-tconst="tt0448115">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt0448115">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt0448115|imdb|7|7|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7/10 (338,061 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="338061" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 98px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt0448115/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">71        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA newly fostered young boy in search of his mother instead finds unexpected super powers and soon gains a powerful enemy.</p>\n    <p class="">\n    Director:\n<a href="/name/nm2497546/"\n>David F. Sandberg</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm1157048/"\n>Zachary Levi</a>, \n<a href="/name/nm0835016/"\n>Mark Strong</a>, \n<a href="/name/nm4755508/"\n>Asher Angel</a>, \n<a href="/name/nm6244013/"\n>Jack Dylan Grazer</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="338061">338,061</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="140,371,656">$140.37M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt4912910" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt4912910/"\n> <img alt="Mission: Impossible - Fallout"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BNjRlZmM0ODktY2RjNS00ZDdjLWJhZGYtNDljNWZkMGM5MTg0XkEyXkFqcGdeQXVyNjAwMjI5MDk@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt4912910"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">31.</span>\n    \n    <a href="/title/tt4912910/"\n>Mission: Impossible - Fallout</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">147 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Thriller            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.7">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.7</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt4912910" data-tconst="tt4912910">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt4912910">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt4912910|imdb|7.7|7.7|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.7/10 (333,149 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.7" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="333149" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 107.8px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.7</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt4912910/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">86        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nEthan Hunt and his IMF team, along with some familiar allies, race against time after a mission gone wrong.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0003160/"\n>Christopher McQuarrie</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000129/"\n>Tom Cruise</a>, \n<a href="/name/nm0147147/"\n>Henry Cavill</a>, \n<a href="/name/nm0000609/"\n>Ving Rhames</a>, \n<a href="/name/nm0670408/"\n>Simon Pegg</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="333149">333,149</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="220,159,104">$220.16M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt2948372" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt2948372/"\n> <img alt="Soul"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BZGE1MDg5M2MtNTkyZS00MTY5LTg1YzUtZTlhZmM1Y2EwNmFmXkEyXkFqcGdeQXVyNjA3OTI0MDc@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt2948372"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">32.</span>\n    \n    <a href="/title/tt2948372/"\n>Soul</a>\n    <span class="lister-item-year text-muted unbold">(2020)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">100 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAnimation, Adventure, Comedy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>8.0</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt2948372" data-tconst="tt2948372">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt2948372">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt2948372|imdb|8|8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 8/10 (328,561 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="328561" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 112px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt2948372/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">83        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nAfter landing the gig of a lifetime, a New York jazz pianist suddenly finds himself trapped in a strange land between Earth and the afterlife.</p>\n    <p class="">\n    Directors:\n<a href="/name/nm0230032/"\n>Pete Docter</a>, \n<a href="/name/nm5358492/"\n>Kemp Powers</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0004937/"\n>Jamie Foxx</a>, \n<a href="/name/nm0275486/"\n>Tina Fey</a>, \n<a href="/name/nm0636218/"\n>Graham Norton</a>, \n<a href="/name/nm1344302/"\n>Rachel House</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="328561">328,561</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt2798920" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt2798920/"\n> <img alt="Annihilation"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTk2Mjc2NzYxNl5BMl5BanBnXkFtZTgwMTA2OTA1NDM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt2798920"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">33.</span>\n    \n    <a href="/title/tt2798920/"\n>Annihilation</a>\n    <span class="lister-item-year text-muted unbold">(I) (2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">115 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAdventure, Drama, Horror            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.8</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt2798920" data-tconst="tt2798920">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt2798920">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt2798920|imdb|6.8|6.8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.8/10 (327,212 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="327212" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 95.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt2798920/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">79        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA biologist signs up for a dangerous, secret expedition into a mysterious zone where the laws of nature don\'t apply.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0307497/"\n>Alex Garland</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000204/"\n>Natalie Portman</a>, \n<a href="/name/nm0000492/"\n>Jennifer Jason Leigh</a>, \n<a href="/name/nm1935086/"\n>Tessa Thompson</a>, \n<a href="/name/nm0938950/"\n>Benedict Wong</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="327212">327,212</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="32,732,301">$32.73M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt8772262" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt8772262/"\n> <img alt="Midsommar"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMzQxNzQzOTQwM15BMl5BanBnXkFtZTgwMDQ2NTcwODM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt8772262"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">34.</span>\n    \n    <a href="/title/tt8772262/"\n>Midsommar</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">148 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nDrama, Horror, Mystery            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.1">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.1</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt8772262" data-tconst="tt8772262">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt8772262">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt8772262|imdb|7.1|7.1|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.1/10 (321,713 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.1" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="321713" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 99.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.1</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt8772262/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">72        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA couple travels to Northern Europe to visit a rural hometown\'s fabled Swedish mid-summer festival. What begins as an idyllic retreat quickly devolves into an increasingly violent and bizarre competition at the hands of a pagan cult.</p>\n    <p class="">\n    Director:\n<a href="/name/nm4170048/"\n>Ari Aster</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm6073955/"\n>Florence Pugh</a>, \n<a href="/name/nm2930503/"\n>Jack Reynor</a>, \n<a href="/name/nm9859585/"\n>Vilhelm Blomgren</a>, \n<a href="/name/nm2860379/"\n>William Jackson Harper</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="321713">321,713</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="27,331,977">$27.33M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt4881806" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt4881806/"\n> <img alt="Jurassic World: Fallen Kingdom"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BNzIxMjYwNDEwN15BMl5BanBnXkFtZTgwMzk5MDI3NTM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt4881806"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">35.</span>\n    \n    <a href="/title/tt4881806/"\n>Jurassic World: Fallen Kingdom</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">128 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.1">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.1</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt4881806" data-tconst="tt4881806">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt4881806">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt4881806|imdb|6.1|6.1|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.1/10 (317,220 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.1" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="317220" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 85.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.1</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt4881806/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">51        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nWhen the island\'s dormant volcano begins roaring to life, Owen and Claire mount a campaign to rescue the remaining dinosaurs from this extinction-level event.</p>\n    <p class="">\n    Director:\n<a href="/name/nm1291105/"\n>J.A. Bayona</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0695435/"\n>Chris Pratt</a>, \n<a href="/name/nm0397171/"\n>Bryce Dallas Howard</a>, \n<a href="/name/nm1245863/"\n>Rafe Spall</a>, \n<a href="/name/nm6819854/"\n>Justice Smith</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="317220">317,220</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="417,719,760">$417.72M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7784604" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7784604/"\n> <img alt="Hereditary"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BOTU5MDg3OGItZWQ1Ny00ZGVmLTg2YTUtMzBkYzQ1YWIwZjlhXkEyXkFqcGdeQXVyNTAzMTY4MDA@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7784604"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">36.</span>\n    \n    <a href="/title/tt7784604/"\n>Hereditary</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">127 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nDrama, Horror, Mystery            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.3">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.3</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7784604" data-tconst="tt7784604">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7784604">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7784604|imdb|7.3|7.3|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.3/10 (316,201 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.3" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="316201" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 102.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.3</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7784604/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">87        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA grieving family is haunted by tragic and disturbing occurrences.</p>\n    <p class="">\n    Director:\n<a href="/name/nm4170048/"\n>Ari Aster</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0001057/"\n>Toni Collette</a>, \n<a href="/name/nm8412536/"\n>Milly Shapiro</a>, \n<a href="/name/nm0000321/"\n>Gabriel Byrne</a>, \n<a href="/name/nm1842974/"\n>Alex Wolff</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="316201">316,201</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="44,069,456">$44.07M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7653254" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7653254/"\n> <img alt="Marriage Story"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BZGVmY2RjNDgtMTc3Yy00YmY0LTgwODItYzBjNWJhNTRlYjdkXkEyXkFqcGdeQXVyMjM4NTM5NDY@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7653254"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">37.</span>\n    \n    <a href="/title/tt7653254/"\n>Marriage Story</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">137 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nDrama, Romance            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.9">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.9</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7653254" data-tconst="tt7653254">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7653254">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7653254|imdb|7.9|7.9|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.9/10 (309,086 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.9" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="309086" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 110.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.9</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7653254/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">94        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nNoah Baumbach\'s incisive and compassionate look at a marriage breaking up and a family staying together.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0000876/"\n>Noah Baumbach</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm3485845/"\n>Adam Driver</a>, \n<a href="/name/nm0424060/"\n>Scarlett Johansson</a>, \n<a href="/name/nm5005121/"\n>Julia Greer</a>, \n<a href="/name/nm7309485/"\n>Azhy Robertson</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="309086">309,086</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="2,000,000">$2.00M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6857112" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6857112/"\n> <img alt="Us"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BZTliNWJhM2YtNDc1MC00YTk1LWE2MGYtZmE4M2Y5ODdlNzQzXkEyXkFqcGdeQXVyMzY0MTE3NzU@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6857112"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">38.</span>\n    \n    <a href="/title/tt6857112/"\n>Us</a>\n    <span class="lister-item-year text-muted unbold">(II) (2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">116 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nHorror, Mystery, Thriller            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.8</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6857112" data-tconst="tt6857112">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6857112">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6857112|imdb|6.8|6.8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.8/10 (299,444 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="299444" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 95.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6857112/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">81        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA family\'s serene beach vacation turns to chaos when their doppelg\xc3\xa4ngers appear and begin to terrorize them.</p>\n    <p class="">\n    Director:\n<a href="/name/nm1443502/"\n>Jordan Peele</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm2143282/"\n>Lupita Nyong\'o</a>, \n<a href="/name/nm6328300/"\n>Winston Duke</a>, \n<a href="/name/nm0005253/"\n>Elisabeth Moss</a>, \n<a href="/name/nm1727367/"\n>Tim Heidecker</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="299444">299,444</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="175,084,580">$175.08M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt3606756" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt3606756/"\n> <img alt="Incredibles 2"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTEzNzY0OTg0NTdeQTJeQWpwZ15BbWU4MDU3OTg3MjUz._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt3606756"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">39.</span>\n    \n    <a href="/title/tt3606756/"\n>Incredibles 2</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">118 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAnimation, Action, Adventure            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.6">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.6</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt3606756" data-tconst="tt3606756">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt3606756">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt3606756|imdb|7.6|7.6|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.6/10 (297,458 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.6" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="297458" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 106.4px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.6</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt3606756/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">80        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nThe Incredibles family takes on a new mission which involves a change in family roles: Bob Parr (Mr. Incredible) must manage the house while his wife Helen (Elastigirl) goes out to save the world.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0083348/"\n>Brad Bird</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0005266/"\n>Craig T. Nelson</a>, \n<a href="/name/nm0000456/"\n>Holly Hunter</a>, \n<a href="/name/nm1102970/"\n>Sarah Vowell</a>, \n<a href="/name/nm9133740/"\n>Huck Milner</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="297458">297,458</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="608,581,744">$608.58M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt4123430" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt4123430/"\n> <img alt="Fantastic Beasts: The Crimes of Grindelwald"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BYWVlMDI5N2UtZTIyMC00NjZkLWI5Y2QtODM5NGE5MzA0NmVjXkEyXkFqcGdeQXVyNzU3NjUxMzE@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt4123430"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">40.</span>\n    \n    <a href="/title/tt4123430/"\n>Fantastic Beasts: The Crimes of Grindelwald</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">134 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAdventure, Family, Fantasy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.5">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.5</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt4123430" data-tconst="tt4123430">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt4123430">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt4123430|imdb|6.5|6.5|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.5/10 (283,595 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.5" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="283595" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 91px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.5</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt4123430/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">52        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nThe second installment of the "Fantastic Beasts" series featuring the adventures of Magizoologist Newt Scamander.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0946734/"\n>David Yates</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm1519666/"\n>Eddie Redmayne</a>, \n<a href="/name/nm2239702/"\n>Katherine Waterston</a>, \n<a href="/name/nm0283945/"\n>Dan Fogler</a>, \n<a href="/name/nm0000136/"\n>Johnny Depp</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="283595">283,595</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="159,555,901">$159.56M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt5727208" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt5727208/"\n> <img alt="Uncut Gems"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BZDhkMjUyYjItYWVkYi00YTM5LWE4MGEtY2FlMjA3OThlYmZhXkEyXkFqcGdeQXVyODk4OTc3MTY@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt5727208"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">41.</span>\n    \n    <a href="/title/tt5727208/"\n>Uncut Gems</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">135 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nCrime, Drama, Thriller            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt5727208" data-tconst="tt5727208">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt5727208">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt5727208|imdb|7.4|7.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.4/10 (282,049 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="282049" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 103.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt5727208/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">91        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nWith his debts mounting and angry collectors closing in, a fast-talking New York City jeweler risks everything in hope of staying afloat and alive.</p>\n    <p class="">\n    Directors:\n<a href="/name/nm1509478/"\n>Benny Safdie</a>, \n<a href="/name/nm1343394/"\n>Josh Safdie</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0001191/"\n>Adam Sandler</a>, \n<a href="/name/nm9681752/"\n>Julia Fox</a>, \n<a href="/name/nm0579953/"\n>Idina Menzel</a>, \n<a href="/name/nm11243726/"\n>Mesfin Lamengo</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="282049">282,049</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt0437086" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt0437086/"\n> <img alt="Alita: Battle Angel"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTQzYWYwYjctY2JhZS00NTYzLTllM2UtZWY5ZTk0NmYwYzIyXkEyXkFqcGdeQXVyMzgxODM4NjM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt0437086"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">42.</span>\n    \n    <a href="/title/tt0437086/"\n>Alita: Battle Angel</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">122 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Sci-Fi            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.3">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.3</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt0437086" data-tconst="tt0437086">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt0437086">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt0437086|imdb|7.3|7.3|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.3/10 (268,519 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.3" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="268519" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 102.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.3</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt0437086/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">53        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA deactivated cyborg\'s revived, but can\'t remember anything of her past and goes on a quest to find out who she is.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0001675/"\n>Robert Rodriguez</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm4023073/"\n>Rosa Salazar</a>, \n<a href="/name/nm0910607/"\n>Christoph Waltz</a>, \n<a href="/name/nm0000124/"\n>Jennifer Connelly</a>, \n<a href="/name/nm0991810/"\n>Mahershala Ali</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="268519">268,519</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="85,710,210">$85.71M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7126948" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7126948/"\n> <img alt="Wonder Woman 1984"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BYTlhNzJjYzYtNGU3My00ZDI5LTgzZDUtYzllYjU1ZmU0YTgwXkEyXkFqcGdeQXVyMjQwMDg0Ng@@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7126948"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">43.</span>\n    \n    <a href="/title/tt7126948/"\n>Wonder Woman 1984</a>\n    <span class="lister-item-year text-muted unbold">(2020)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">151 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Fantasy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="5.4">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>5.4</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7126948" data-tconst="tt7126948">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7126948">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7126948|imdb|5.4|5.4|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 5.4/10 (267,268 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="5.4" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="267268" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 75.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">5.4</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7126948/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">60        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nDiana must contend with a work colleague, and with a businessman whose desire for extreme wealth sends the world down a path of destruction, after an ancient artifact that grants wishes goes missing.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0420941/"\n>Patty Jenkins</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm2933757/"\n>Gal Gadot</a>, \n<a href="/name/nm1517976/"\n>Chris Pine</a>, \n<a href="/name/nm1325419/"\n>Kristen Wiig</a>, \n<a href="/name/nm0050959/"\n>Pedro Pascal</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="267268">267,268</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="46,370,364">$46.37M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7349950" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7349950/"\n> <img alt="It Chapter Two"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BYTJlNjlkZTktNjEwOS00NzI5LTlkNDAtZmEwZDFmYmM2MjU2XkEyXkFqcGdeQXVyNjg2NjQwMDQ@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7349950"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">44.</span>\n    \n    <a href="/title/tt7349950/"\n>It Chapter Two</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">169 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nDrama, Fantasy, Horror            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.5">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.5</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7349950" data-tconst="tt7349950">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7349950">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7349950|imdb|6.5|6.5|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.5/10 (266,321 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.5" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="266321" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 91px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.5</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7349950/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">58        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nTwenty-seven years after their first encounter with the terrifying Pennywise, the Losers Club have grown up and moved away, until a devastating phone call brings them back.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0615592/"\n>Andy Muschietti</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm1567113/"\n>Jessica Chastain</a>, \n<a href="/name/nm0564215/"\n>James McAvoy</a>, \n<a href="/name/nm0352778/"\n>Bill Hader</a>, \n<a href="/name/nm2248149/"\n>Isaiah Mustafa</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="266321">266,321</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="211,593,228">$211.59M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6139732" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6139732/"\n> <img alt="Aladdin"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMjQ2ODIyMjY4MF5BMl5BanBnXkFtZTgwNzY4ODI2NzM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6139732"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">45.</span>\n    \n    <a href="/title/tt6139732/"\n>Aladdin</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">128 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAdventure, Comedy, Family            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.9">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.9</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6139732" data-tconst="tt6139732">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6139732">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6139732|imdb|6.9|6.9|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.9/10 (266,235 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.9" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="266235" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 96.6px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.9</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6139732/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">53        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nA kind-hearted street urchin and a power-hungry Grand Vizier vie for a magic lamp that has the power to make their deepest wishes come true.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0005363/"\n>Guy Ritchie</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000226/"\n>Will Smith</a>, \n<a href="/name/nm4565815/"\n>Mena Massoud</a>, \n<a href="/name/nm4305463/"\n>Naomi Scott</a>, \n<a href="/name/nm3092471/"\n>Marwan Kenzari</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="266235">266,235</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="355,559,216">$355.56M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7349662" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7349662/"\n> <img alt="BlacKkKlansman"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMjUyOTE1NjI0OF5BMl5BanBnXkFtZTgwMTM4ODQ5NTM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7349662"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">46.</span>\n    \n    <a href="/title/tt7349662/"\n>BlacKkKlansman</a>\n    <span class="lister-item-year text-muted unbold">(2018)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">R</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">135 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nBiography, Comedy, Crime            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.5">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.5</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7349662" data-tconst="tt7349662">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7349662">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7349662|imdb|7.5|7.5|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.5/10 (264,596 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.5" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="264596" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 105px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.5</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7349662/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">83        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\n<a href="/name/nm9259302">Ron Stallworth</a>, an African American police officer from Colorado Springs, CO, successfully manages to infiltrate the local Ku Klux Klan branch with the help of a Jewish surrogate who eventually becomes its leader. Based on actual events.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0000490/"\n>Spike Lee</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0913475/"\n>John David Washington</a>, \n<a href="/name/nm3485845/"\n>Adam Driver</a>, \n<a href="/name/nm5637553/"\n>Laura Harrier</a>, \n<a href="/name/nm0333410/"\n>Topher Grace</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="264596">264,596</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="49,275,340">$49.28M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt1979376" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt1979376/"\n> <img alt="Toy Story 4"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMTYzMDM4NzkxOV5BMl5BanBnXkFtZTgwNzM1Mzg2NzM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt1979376"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">47.</span>\n    \n    <a href="/title/tt1979376/"\n>Toy Story 4</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">G</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">100 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAnimation, Adventure, Comedy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.7">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.7</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt1979376" data-tconst="tt1979376">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt1979376">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt1979376|imdb|7.7|7.7|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.7/10 (250,943 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.7" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="250943" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 107.8px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.7</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt1979376/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">84        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nWhen a new toy called "Forky" joins Woody and the gang, a road trip alongside old and new friends reveals how big the world can be for a toy.</p>\n    <p class="">\n    Director:\n<a href="/name/nm2155757/"\n>Josh Cooley</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0000158/"\n>Tom Hanks</a>, \n<a href="/name/nm0000741/"\n>Tim Allen</a>, \n<a href="/name/nm0001633/"\n>Annie Potts</a>, \n<a href="/name/nm0355024/"\n>Tony Hale</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="250943">250,943</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="434,038,008">$434.04M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt7975244" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt7975244/"\n> <img alt="Jumanji: The Next Level"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BOTVjMmFiMDUtOWQ4My00YzhmLWE3MzEtODM1NDFjMWEwZTRkXkEyXkFqcGdeQXVyMTkxNjUyNQ@@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt7975244"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">48.</span>\n    \n    <a href="/title/tt7975244/"\n>Jumanji: The Next Level</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG-13</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">123 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Adventure, Comedy            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.7">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.7</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt7975244" data-tconst="tt7975244">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt7975244">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt7975244|imdb|6.7|6.7|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.7/10 (250,934 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.7" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="250934" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 93.8px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.7</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt7975244/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">58        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nIn Jumanji: The Next Level, the gang is back but the game has changed. As they return to rescue one of their own, the players will have to brave parts unknown from arid deserts to snowy mountains, to escape the world\'s most dangerous game.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0440458/"\n>Jake Kasdan</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0425005/"\n>Dwayne Johnson</a>, \n<a href="/name/nm0085312/"\n>Jack Black</a>, \n<a href="/name/nm0366389/"\n>Kevin Hart</a>, \n<a href="/name/nm2394794/"\n>Karen Gillan</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="250934">250,934</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="316,831,246">$316.83M</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt9243946" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt9243946/"\n> <img alt="El Camino: A Breaking Bad Movie"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BNjk4MzVlM2UtZGM0ZC00M2M1LThkMWEtZjUyN2U2ZTc0NmM5XkEyXkFqcGdeQXVyOTAzMTc2MjA@._V1_UY98_CR5,0,67,98_AL_.jpg"\ndata-tconst="tt9243946"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">49.</span>\n    \n    <a href="/title/tt9243946/"\n>El Camino: A Breaking Bad Movie</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">TV-MA</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">122 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAction, Crime, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="7.3">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>7.3</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt9243946" data-tconst="tt9243946">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt9243946">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt9243946|imdb|7.3|7.3|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 7.3/10 (250,280 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="7.3" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="250280" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 102.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">7.3</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt9243946/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  favorable">72        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nFugitive Jesse Pinkman runs from his captors, the law, and his past.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0319213/"\n>Vince Gilligan</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm0666739/"\n>Aaron Paul</a>, \n<a href="/name/nm0052186/"\n>Jonathan Banks</a>, \n<a href="/name/nm2804503/"\n>Matt Jones</a>, \n<a href="/name/nm1889973/"\n>Charles Baker</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="250280">250,280</span>\n            \n        </p>\n        </div>\n    </div>\n\n    <div class="lister-item mode-advanced">\n        <div class="lister-top-right">\n    <div class="ribbonize" data-tconst="tt6105098" data-caller="filmosearch"></div>\n        </div>\n        <div class="lister-item-image float-left">\n\n\n<a href="/title/tt6105098/"\n> <img alt="The Lion King"\nclass="loadlate"\nloadlate="https://m.media-amazon.com/images/M/MV5BMjIwMjE1Nzc4NV5BMl5BanBnXkFtZTgwNDg4OTA1NzM@._V1_UX67_CR0,0,67,98_AL_.jpg"\ndata-tconst="tt6105098"\nheight="98"\nsrc="https://m.media-amazon.com/images/S/sash/4FyxwxECzL-U1J8.png"\nwidth="67" />\n</a>        </div>\n        <div class="lister-item-content">\n<h3 class="lister-item-header">\n        <span class="lister-item-index unbold text-primary">50.</span>\n    \n    <a href="/title/tt6105098/"\n>The Lion King</a>\n    <span class="lister-item-year text-muted unbold">(2019)</span>\n</h3>\n    <p class="text-muted ">\n            <span class="certificate">PG</span>\n                 <span class="ghost">|</span> \n                <span class="runtime">118 min</span>\n                 <span class="ghost">|</span> \n            <span class="genre">\nAnimation, Adventure, Drama            </span>\n    </p>\n    <div class="ratings-bar">\n    <div class="inline-block ratings-imdb-rating" name="ir" data-value="6.8">\n        <span class="global-sprite rating-star imdb-rating"></span>\n        <strong>6.8</strong>\n    </div>\n            <div class="inline-block ratings-user-rating">\n                <span class="userRatingValue" id="urv_tt6105098" data-tconst="tt6105098">\n                    <span class="global-sprite rating-star no-rating"></span>\n                    <span name="ur" data-value="0" class="rate" data-no-rating="Rate this">Rate this</span>\n                </span>\n    <div class="starBarWidget" id="sb_tt6105098">\n<div class="rating rating-list" data-starbar-class="rating-list" data-csrf-token="" data-user="" id="tt6105098|imdb|6.8|6.8|adv_li_tt||advsearch|title" data-ga-identifier=""\ntitle="Users rated this 6.8/10 (245,586 votes) - click stars to rate" itemtype="http://schema.org/AggregateRating" itemscope itemprop="aggregateRating">\n  <meta itemprop="ratingValue" content="6.8" />\n  <meta itemprop="bestRating" content="10" />\n  <meta itemprop="ratingCount" content="245586" />\n<span class="rating-bg">&nbsp;</span>\n<span class="rating-imdb " style="width: 95.2px">&nbsp;</span>\n<span class="rating-stars">\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>1</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>2</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>3</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>4</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>5</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>6</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>7</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>8</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>9</span></a>\n      <a href="/register/login?why=vote"\nrel="nofollow" title="Register or login to rate this title" ><span>10</span></a>\n</span>\n<span class="rating-rating "><span class="value">6.8</span><span class="grey">/</span><span class="grey">10</span></span>\n<span class="rating-cancel "><a href="/title/tt6105098/vote" title="Delete" rel="nofollow"><span>X</span></a></span>\n&nbsp;</div>\n    </div>\n            </div>\n            <div class="inline-block ratings-metascore">\n<span class="metascore  mixed">55        </span>\n        Metascore\n            </div>\n    </div>\n<p class="text-muted">\nAfter the murder of his father, a young lion prince flees his kingdom only to learn the true meaning of responsibility and bravery.</p>\n    <p class="">\n    Director:\n<a href="/name/nm0269463/"\n>Jon Favreau</a>\n                 <span class="ghost">|</span> \n    Stars:\n<a href="/name/nm2255973/"\n>Donald Glover</a>, \n<a href="/name/nm0461498/"\n>Beyonc\xc3\xa9</a>, \n<a href="/name/nm0736622/"\n>Seth Rogen</a>, \n<a href="/name/nm0252230/"\n>Chiwetel Ejiofor</a>\n    </p>\n        <p class="sort-num_votes-visible">\n                <span class="text-muted">Votes:</span>\n                <span name="nv" data-value="245586">245,586</span>\n    <span class="ghost">|</span>                <span class="text-muted">Gross:</span>\n                <span name="nv" data-value="543,638,043">$543.64M</span>\n            \n        </p>\n        </div>\n    </div>\n         </div>\n     </div>\n                <br class="clear" />\n    <div class="desc">\n        <span>1-50 of 35,913 titles.</span>\n    <span class="ghost">|</span>        <a href="/search/title/?title_type=feature&year=2018-01-01,2020-12-31&sort=num_votes,desc&start=51"\nclass="lister-page-next next-page" >Next &#187;</a>\n    </div>\n        </div>\n    </div>\n    <div id="sidebar">\n\t\n        <!-- no content received for slot: top_rhs -->\n            <script>\n                var event = {\n                    type: \'\',\n                    slotName: \'top_rhs\',\n                    timestamp: Date.now()\n                };\n                var mediaEvent = event;\n                mediaEvent.type = \'no-autoplay-video-ad-detected\';\n                if (window && window.mediaOrchestrator) {\n                    window.mediaOrchestrator.publish(\'mediaPlaybackEvent\', mediaEvent);\n                    window.mediaOrchestrator.publish(\'noAdToLoad\', event);\n                }\n            </script>\n\t\n    </div>\n</div>\n\n\n                  <br class="clear" />\n               </div>\n\n\n                    <div id="rvi-div">\n                        <div class="recently-viewed">\n                        <div class="header">\n                            <div class="rhs">\n                                <a id="clear_rvi" href="#">Clear your history</a>\n                            </div>\n                            <h3>Recently Viewed</h3>\n                        </div>\n                            <div class="items">&nbsp;</div>\n                        </div>\n                    </div>\n\n\t\n        <!-- no content received for slot: bottom_ad -->\n\t\n\n            </div>\n        </div>\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "desktopFooter", {wb: 1});\n    }\n</script>\n\n\n<style data-styled="true" data-styled-version="5.3.6">.dvqHbV{--ipt-base-rgb:255,255,255;--mdc-theme-ipt-base-rgb:var(--ipt-base-rgb);--ipt-base-bg:rgb(255,255,255);--mdc-theme-ipt-base-bg:var(--ipt-base-bg);--ipt-base-color:rgb(255,255,255);--mdc-theme-ipt-base-color:var(--ipt-base-color);--ipt-base-shade1-rgb:250,250,250;--mdc-theme-ipt-base-shade1-rgb:var(--ipt-base-shade1-rgb);--ipt-base-shade1-bg:rgb(250,250,250);--mdc-theme-ipt-base-shade1-bg:var(--ipt-base-shade1-bg);--ipt-base-shade1-color:rgb(250,250,250);--mdc-theme-ipt-base-shade1-color:var(--ipt-base-shade1-color);--ipt-base-shade2-rgb:240,240,240;--mdc-theme-ipt-base-shade2-rgb:var(--ipt-base-shade2-rgb);--ipt-base-shade2-bg:rgb(240,240,240);--mdc-theme-ipt-base-shade2-bg:var(--ipt-base-shade2-bg);--ipt-base-shade2-color:rgb(240,240,240);--mdc-theme-ipt-base-shade2-color:var(--ipt-base-shade2-color);--ipt-base-shade3-rgb:255,255,255;--mdc-theme-ipt-base-shade3-rgb:var(--ipt-base-shade3-rgb);--ipt-base-shade3-bg:rgb(255,255,255);--mdc-theme-ipt-base-shade3-bg:var(--ipt-base-shade3-bg);--ipt-base-shade3-color:rgb(255,255,255);--mdc-theme-ipt-base-shade3-color:var(--ipt-base-shade3-color);--ipt-on-base-rgb:0,0,0;--mdc-theme-ipt-on-base-rgb:var(--ipt-on-base-rgb);--ipt-on-base-color:rgb(0,0,0);--mdc-theme-ipt-on-base-color:var(--ipt-on-base-color);--ipt-on-base-accent1-rgb:245,197,24;--mdc-theme-ipt-on-base-accent1-rgb:var(--ipt-on-base-accent1-rgb);--ipt-on-base-accent1-color:rgb(245,197,24);--mdc-theme-ipt-on-base-accent1-color:var(--ipt-on-base-accent1-color);--ipt-on-base-accent2-rgb:14,99,190;--mdc-theme-ipt-on-base-accent2-rgb:var(--ipt-on-base-accent2-rgb);--ipt-on-base-accent2-color:rgb(14,99,190);--mdc-theme-ipt-on-base-accent2-color:var(--ipt-on-base-accent2-color);--ipt-on-base-accent3-rgb:189,36,4;--mdc-theme-ipt-on-base-accent3-rgb:var(--ipt-on-base-accent3-rgb);--ipt-on-base-accent3-color:rgb(189,36,4);--mdc-theme-ipt-on-base-accent3-color:var(--ipt-on-base-accent3-color);--ipt-on-base-accent4-rgb:0,138,0;--mdc-theme-ipt-on-base-accent4-rgb:var(--ipt-on-base-accent4-rgb);--ipt-on-base-accent4-color:rgb(0,138,0);--mdc-theme-ipt-on-base-accent4-color:var(--ipt-on-base-accent4-color);--ipt-on-base-textPrimary-opacity:0.87;--mdc-theme-ipt-on-base-textPrimary-opacity:var(--ipt-on-base-textPrimary-opacity);--ipt-on-base-textPrimary-color:rgba(0,0,0,0.87);--mdc-theme-ipt-on-base-textPrimary-color:var(--ipt-on-base-textPrimary-color);--ipt-on-base-textSecondary-opacity:0.54;--mdc-theme-ipt-on-base-textSecondary-opacity:var(--ipt-on-base-textSecondary-opacity);--ipt-on-base-textSecondary-color:rgba(0,0,0,0.54);--mdc-theme-ipt-on-base-textSecondary-color:var(--ipt-on-base-textSecondary-color);--ipt-on-base-textHint-opacity:0.38;--mdc-theme-ipt-on-base-textHint-opacity:var(--ipt-on-base-textHint-opacity);--ipt-on-base-textHint-color:rgba(0,0,0,0.38);--mdc-theme-ipt-on-base-textHint-color:var(--ipt-on-base-textHint-color);--ipt-on-base-textDisabled-opacity:0.38;--mdc-theme-ipt-on-base-textDisabled-opacity:var(--ipt-on-base-textDisabled-opacity);--ipt-on-base-textDisabled-color:rgba(0,0,0,0.38);--mdc-theme-ipt-on-base-textDisabled-color:var(--ipt-on-base-textDisabled-color);--ipt-on-base-textIcon-opacity:0.5;--mdc-theme-ipt-on-base-textIcon-opacity:var(--ipt-on-base-textIcon-opacity);--ipt-on-base-textIcon-color:rgba(0,0,0,0.5);--mdc-theme-ipt-on-base-textIcon-color:var(--ipt-on-base-textIcon-color);--ipt-base-border-opacity:0.12;--mdc-theme-ipt-base-border-opacity:var(--ipt-base-border-opacity);--ipt-base-hover-opacity:0.08;--mdc-theme-ipt-base-hover-opacity:var(--ipt-base-hover-opacity);--ipt-base-pressed-opacity:0.16;--mdc-theme-ipt-base-pressed-opacity:var(--ipt-base-pressed-opacity);--ipt-base-stripes-opacity:0.04;--mdc-theme-ipt-base-stripes-opacity:var(--ipt-base-stripes-opacity);--ipt-base-border-color:rgba(0,0,0,0.12);--mdc-theme-ipt-base-border-color:var(--ipt-base-border-color);--ipt-base-hover-color:rgba(0,0,0,0.08);--mdc-theme-ipt-base-hover-color:var(--ipt-base-hover-color);--ipt-base-pressed-color:rgba(0,0,0,0.16);--mdc-theme-ipt-base-pressed-color:var(--ipt-base-pressed-color);--ipt-base-stripes-color:rgba(0,0,0,0.04);--mdc-theme-ipt-base-stripes-color:var(--ipt-base-stripes-color);--ipt-baseAlt-rgb:0,0,0;--mdc-theme-ipt-baseAlt-rgb:var(--ipt-baseAlt-rgb);--ipt-baseAlt-bg:rgb(0,0,0);--mdc-theme-ipt-baseAlt-bg:var(--ipt-baseAlt-bg);--ipt-baseAlt-color:rgb(0,0,0);--mdc-theme-ipt-baseAlt-color:var(--ipt-baseAlt-color);--ipt-baseAlt-shade1-rgb:31,31,31;--mdc-theme-ipt-baseAlt-shade1-rgb:var(--ipt-baseAlt-shade1-rgb);--ipt-baseAlt-shade1-bg:rgb(31,31,31);--mdc-theme-ipt-baseAlt-shade1-bg:var(--ipt-baseAlt-shade1-bg);--ipt-baseAlt-shade1-color:rgb(31,31,31);--mdc-theme-ipt-baseAlt-shade1-color:var(--ipt-baseAlt-shade1-color);--ipt-baseAlt-shade2-rgb:26,26,26;--mdc-theme-ipt-baseAlt-shade2-rgb:var(--ipt-baseAlt-shade2-rgb);--ipt-baseAlt-shade2-bg:rgb(26,26,26);--mdc-theme-ipt-baseAlt-shade2-bg:var(--ipt-baseAlt-shade2-bg);--ipt-baseAlt-shade2-color:rgb(26,26,26);--mdc-theme-ipt-baseAlt-shade2-color:var(--ipt-baseAlt-shade2-color);--ipt-baseAlt-shade3-rgb:18,18,18;--mdc-theme-ipt-baseAlt-shade3-rgb:var(--ipt-baseAlt-shade3-rgb);--ipt-baseAlt-shade3-bg:rgb(18,18,18);--mdc-theme-ipt-baseAlt-shade3-bg:var(--ipt-baseAlt-shade3-bg);--ipt-baseAlt-shade3-color:rgb(18,18,18);--mdc-theme-ipt-baseAlt-shade3-color:var(--ipt-baseAlt-shade3-color);--ipt-on-baseAlt-rgb:255,255,255;--mdc-theme-ipt-on-baseAlt-rgb:var(--ipt-on-baseAlt-rgb);--ipt-on-baseAlt-color:rgb(255,255,255);--mdc-theme-ipt-on-baseAlt-color:var(--ipt-on-baseAlt-color);--ipt-on-baseAlt-accent1-rgb:245,197,24;--mdc-theme-ipt-on-baseAlt-accent1-rgb:var(--ipt-on-baseAlt-accent1-rgb);--ipt-on-baseAlt-accent1-color:rgb(245,197,24);--mdc-theme-ipt-on-baseAlt-accent1-color:var(--ipt-on-baseAlt-accent1-color);--ipt-on-baseAlt-accent2-rgb:87,153,239;--mdc-theme-ipt-on-baseAlt-accent2-rgb:var(--ipt-on-baseAlt-accent2-rgb);--ipt-on-baseAlt-accent2-color:rgb(87,153,239);--mdc-theme-ipt-on-baseAlt-accent2-color:var(--ipt-on-baseAlt-accent2-color);--ipt-on-baseAlt-accent3-rgb:251,60,60;--mdc-theme-ipt-on-baseAlt-accent3-rgb:var(--ipt-on-baseAlt-accent3-rgb);--ipt-on-baseAlt-accent3-color:rgb(251,60,60);--mdc-theme-ipt-on-baseAlt-accent3-color:var(--ipt-on-baseAlt-accent3-color);--ipt-on-baseAlt-accent4-rgb:103,173,75;--mdc-theme-ipt-on-baseAlt-accent4-rgb:var(--ipt-on-baseAlt-accent4-rgb);--ipt-on-baseAlt-accent4-color:rgb(103,173,75);--mdc-theme-ipt-on-baseAlt-accent4-color:var(--ipt-on-baseAlt-accent4-color);--ipt-on-baseAlt-textPrimary-opacity:1;--mdc-theme-ipt-on-baseAlt-textPrimary-opacity:var(--ipt-on-baseAlt-textPrimary-opacity);--ipt-on-baseAlt-textPrimary-color:rgba(255,255,255,1);--mdc-theme-ipt-on-baseAlt-textPrimary-color:var(--ipt-on-baseAlt-textPrimary-color);--ipt-on-baseAlt-textSecondary-opacity:0.7;--mdc-theme-ipt-on-baseAlt-textSecondary-opacity:var(--ipt-on-baseAlt-textSecondary-opacity);--ipt-on-baseAlt-textSecondary-color:rgba(255,255,255,0.7);--mdc-theme-ipt-on-baseAlt-textSecondary-color:var(--ipt-on-baseAlt-textSecondary-color);--ipt-on-baseAlt-textHint-opacity:0.5;--mdc-theme-ipt-on-baseAlt-textHint-opacity:var(--ipt-on-baseAlt-textHint-opacity);--ipt-on-baseAlt-textHint-color:rgba(255,255,255,0.5);--mdc-theme-ipt-on-baseAlt-textHint-color:var(--ipt-on-baseAlt-textHint-color);--ipt-on-baseAlt-textDisabled-opacity:0.5;--mdc-theme-ipt-on-baseAlt-textDisabled-opacity:var(--ipt-on-baseAlt-textDisabled-opacity);--ipt-on-baseAlt-textDisabled-color:rgba(255,255,255,0.5);--mdc-theme-ipt-on-baseAlt-textDisabled-color:var(--ipt-on-baseAlt-textDisabled-color);--ipt-on-baseAlt-textIcon-opacity:1;--mdc-theme-ipt-on-baseAlt-textIcon-opacity:var(--ipt-on-baseAlt-textIcon-opacity);--ipt-on-baseAlt-textIcon-color:rgba(255,255,255,1);--mdc-theme-ipt-on-baseAlt-textIcon-color:var(--ipt-on-baseAlt-textIcon-color);--ipt-baseAlt-border-opacity:0.2;--mdc-theme-ipt-baseAlt-border-opacity:var(--ipt-baseAlt-border-opacity);--ipt-baseAlt-hover-opacity:0.08;--mdc-theme-ipt-baseAlt-hover-opacity:var(--ipt-baseAlt-hover-opacity);--ipt-baseAlt-pressed-opacity:0.32;--mdc-theme-ipt-baseAlt-pressed-opacity:var(--ipt-baseAlt-pressed-opacity);--ipt-baseAlt-stripes-opacity:0.12;--mdc-theme-ipt-baseAlt-stripes-opacity:var(--ipt-baseAlt-stripes-opacity);--ipt-baseAlt-border-color:rgba(255,255,255,0.2);--mdc-theme-ipt-baseAlt-border-color:var(--ipt-baseAlt-border-color);--ipt-baseAlt-hover-color:rgba(255,255,255,0.08);--mdc-theme-ipt-baseAlt-hover-color:var(--ipt-baseAlt-hover-color);--ipt-baseAlt-pressed-color:rgba(255,255,255,0.32);--mdc-theme-ipt-baseAlt-pressed-color:var(--ipt-baseAlt-pressed-color);--ipt-baseAlt-stripes-color:rgba(255,255,255,0.12);--mdc-theme-ipt-baseAlt-stripes-color:var(--ipt-baseAlt-stripes-color);--ipt-accent1-rgb:245,197,24;--mdc-theme-ipt-accent1-rgb:var(--ipt-accent1-rgb);--ipt-accent1-bg:rgb(245,197,24);--mdc-theme-ipt-accent1-bg:var(--ipt-accent1-bg);--ipt-accent1-color:rgb(245,197,24);--mdc-theme-ipt-accent1-color:var(--ipt-accent1-color);--ipt-on-accent1-rgb:0,0,0;--mdc-theme-ipt-on-accent1-rgb:var(--ipt-on-accent1-rgb);--ipt-on-accent1-color:rgb(0,0,0);--mdc-theme-ipt-on-accent1-color:var(--ipt-on-accent1-color);--ipt-accent2-rgb:14,99,190;--mdc-theme-ipt-accent2-rgb:var(--ipt-accent2-rgb);--ipt-accent2-bg:rgb(14,99,190);--mdc-theme-ipt-accent2-bg:var(--ipt-accent2-bg);--ipt-accent2-color:rgb(14,99,190);--mdc-theme-ipt-accent2-color:var(--ipt-accent2-color);--ipt-on-accent2-rgb:255,255,255;--mdc-theme-ipt-on-accent2-rgb:var(--ipt-on-accent2-rgb);--ipt-on-accent2-color:rgb(255,255,255);--mdc-theme-ipt-on-accent2-color:var(--ipt-on-accent2-color);--ipt-accent3-rgb:189,36,4;--mdc-theme-ipt-accent3-rgb:var(--ipt-accent3-rgb);--ipt-accent3-bg:rgb(189,36,4);--mdc-theme-ipt-accent3-bg:var(--ipt-accent3-bg);--ipt-accent3-color:rgb(189,36,4);--mdc-theme-ipt-accent3-color:var(--ipt-accent3-color);--ipt-on-accent3-rgb:255,255,255;--mdc-theme-ipt-on-accent3-rgb:var(--ipt-on-accent3-rgb);--ipt-on-accent3-color:rgb(255,255,255);--mdc-theme-ipt-on-accent3-color:var(--ipt-on-accent3-color);--ipt-accent4-rgb:0,114,7;--mdc-theme-ipt-accent4-rgb:var(--ipt-accent4-rgb);--ipt-accent4-bg:rgb(0,114,7);--mdc-theme-ipt-accent4-bg:var(--ipt-accent4-bg);--ipt-accent4-color:rgb(0,114,7);--mdc-theme-ipt-accent4-color:var(--ipt-accent4-color);--ipt-on-accent4-rgb:255,255,255;--mdc-theme-ipt-on-accent4-rgb:var(--ipt-on-accent4-rgb);--ipt-on-accent4-color:rgb(255,255,255);--mdc-theme-ipt-on-accent4-color:var(--ipt-on-accent4-color);--ipc-pageSection-bottomMargin:.75rem;--mdc-theme-ipc-pageSection-bottomMargin:var(--ipc-pageSection-bottomMargin);--ipc-pageSection-base-bg:rgb(250,250,250);--mdc-theme-ipc-pageSection-base-bg:var(--ipc-pageSection-base-bg);--ipc-pageSection-base-rgb:250,250,250;--mdc-theme-ipc-pageSection-base-rgb:var(--ipc-pageSection-base-rgb);--ipc-pageSection-baseAlt-bg:rgb(18,18,18);--mdc-theme-ipc-pageSection-baseAlt-bg:var(--ipc-pageSection-baseAlt-bg);--ipc-pageSection-baseAlt-rgb:18,18,18;--mdc-theme-ipc-pageSection-baseAlt-rgb:var(--ipc-pageSection-baseAlt-rgb);--ipc-listCard-base-bg:rgb(255,255,255);--mdc-theme-ipc-listCard-base-bg:var(--ipc-listCard-base-bg);--ipc-listCard-base-rgb:255,255,255;--mdc-theme-ipc-listCard-base-rgb:var(--ipc-listCard-base-rgb);--ipc-listCard-baseAlt-bg:rgb(26,26,26);--mdc-theme-ipc-listCard-baseAlt-bg:var(--ipc-listCard-baseAlt-bg);--ipc-listCard-baseAlt-rgb:26,26,26;--mdc-theme-ipc-listCard-baseAlt-rgb:var(--ipc-listCard-baseAlt-rgb);--ipc-prompt-bg:rgb(31,31,31);--mdc-theme-ipc-prompt-bg:var(--ipc-prompt-bg);--ipc-prompt-rgb:31,31,31;--mdc-theme-ipc-prompt-rgb:var(--ipc-prompt-rgb);--mdc-theme-primary:var(--ipt-baseAlt-color);--mdc-theme-secondary:var(--ipt-accent1-color);--mdc-theme-background:var(--ipt-base-color);--mdc-theme-surface:var(--ipt-base-shade1-color);--mdc-theme-on-primary:var(--ipt-on-baseAlt-color);--mdc-theme-on-secondary:var(--ipt-on-accent1-color);--mdc-theme-on-surface:var(--ipt-on-base-color);--mdc-theme-text-primary-on-background:var(--ipt-on-base-textPrimary-color);--mdc-theme-text-secondary-on-background:var(--ipt-on-base-textSecondary-color);--mdc-theme-text-hint-on-background:var(--ipt-on-base-textHint-color);--mdc-theme-text-disabled-on-background:var(--ipt-on-base-textDisabled-color);--mdc-theme-text-icon-on-background:var(--ipt-on-base-textIcon-color);--mdc-theme-text-primary-on-light:var(--ipt-on-base-textPrimary-color);--mdc-theme-text-secondary-on-light:var(--ipt-on-base-textSecondary-color);--mdc-theme-text-hint-on-light:var(--ipt-on-base-textHint-color);--mdc-theme-text-disabled-on-light:var(--ipt-on-base-textDisabled-color);--mdc-theme-text-icon-on-light:var(--ipt-on-base-textIcon-color);--mdc-theme-text-primary-on-dark:var(--ipt-on-baseAlt-textPrimary-color);--mdc-theme-text-secondary-on-dark:var(--ipt-on-baseAlt-textSecondary-color);--mdc-theme-text-hint-on-dark:var(--ipt-on-baseAlt-textHint-color);--mdc-theme-text-disabled-on-dark:var(--ipt-on-baseAlt-textDisabled-color);--mdc-theme-text-icon-on-dark:var(--ipt-on-baseAlt-textIcon-color);background:rgb(0,0,0);background:var(--ipt-baseAlt-bg,rgb(0,0,0));color:rgb(255,255,255);color:var(--ipt-on-baseAlt-color,rgb(255,255,255));font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:1rem;font-size:var(--ipt-type-body-size,1rem);font-weight:400;font-weight:var(--ipt-type-body-weight,400);-webkit-letter-spacing:.03125em;-moz-letter-spacing:.03125em;-ms-letter-spacing:.03125em;letter-spacing:.03125em;-webkit-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-moz-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);-ms-letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);letter-spacing:var(--ipt-type-body-letterSpacing,.03125em);line-height:1.5rem;line-height:var(--ipt-type-body-lineHeight,1.5rem);text-transform:none;text-transform:var(--ipt-type-body-textTransform,none);margin-top:1rem;padding:1rem 0;text-align:center;}/*!sc*/\n.dvqHbV .footer__app{background:rgba(255,255,255,.1);background:rgba(var(--ipt-on-baseAlt-rgb),.1);margin-bottom:.5rem;padding:1rem 0;}/*!sc*/\n.dvqHbV .footer__socials{margin:16px 0;}/*!sc*/\n.dvqHbV .footer__logo{min-height:40px;display:-webkit-box;display:-webkit-flex;display:-ms-flexbox;display:flex;-webkit-box-pack:center;-webkit-justify-content:center;-ms-flex-pack:center;justify-content:center;-webkit-align-items:center;-webkit-box-align:center;-ms-flex-align:center;align-items:center;margin:16px;}/*!sc*/\n.dvqHbV .footer__copyright{color:rgba(255,255,255,0.7);color:var(--ipt-on-baseAlt-textSecondary-color,rgba(255,255,255,0.7));font-family:\'Roboto\',\'Helvetica\',\'Arial\',sans-serif;font-family:var(--ipt-font-family);font-size:.75rem;font-size:var(--ipt-type-copyright-size,.75rem);font-weight:400;font-weight:var(--ipt-type-copyright-weight,400);-webkit-letter-spacing:.03333em;-moz-letter-spacing:.03333em;-ms-letter-spacing:.03333em;letter-spacing:.03333em;-webkit-letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);-moz-letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);-ms-letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);letter-spacing:var(--ipt-type-copyright-letterSpacing,.03333em);line-height:1rem;line-height:var(--ipt-type-copyright-lineHeight,1rem);text-transform:none;text-transform:var(--ipt-type-copyright-textTransform,none);margin:.5em 0;}/*!sc*/\n.dvqHbV .footer__links{max-width:360px;}/*!sc*/\n.dvqHbV .footer__linksRow{display:inline;}/*!sc*/\n.dvqHbV .footer__linksRow li{margin-left:0.5rem;margin-right:0.5rem;}/*!sc*/\n.dvqHbV .footerLink--wideScreen{display:none;}/*!sc*/\n@media screen and (min-width:600px){.dvqHbV{margin-top:0;}.dvqHbV .footer__links{max-width:600px;}.dvqHbV .footer__linksRow{display:inline;}}/*!sc*/\n.dvqHbV .footer__sign-in-button{display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;}/*!sc*/\n.dvqHbV .footer__sign-in-mobile-button{display:none;}/*!sc*/\n@media screen and (max-width:1024px){.dvqHbV .footer__app{margin:0;}.dvqHbV .footer__sign-in{background:rgba( 255,255,255, 0.06 );padding:1rem 0;}.dvqHbV .footer__sign-in-button{display:none;}.dvqHbV .footer__sign-in-mobile-button{display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;}}/*!sc*/\n@media screen and (min-width:1024px){.dvqHbV .footer__links{max-width:1024px;}.dvqHbV .footer__app{display:none;}.dvqHbV .footerLink--wideScreen{display:inline-block;}.dvqHbV .footerLink--smallScreen{display:none;}.dvqHbV .footer__sign-in{background:rgb(0,0,0);background:var(--ipt-baseAlt-bg,rgb(0,0,0));padding:0;}.dvqHbV .footer__sign-in-button{display:-webkit-inline-box;display:-webkit-inline-flex;display:-ms-inline-flexbox;display:inline-flex;}.dvqHbV .footer__sign-in-mobile-button{display:none;}}/*!sc*/\n.dvqHbV .footerLink--trustarc a:hover{color:inherit;-webkit-text-decoration:underline;text-decoration:underline;}/*!sc*/\ndata-styled.g1[id="sc-bcXHqe"]{content:"dvqHbV,"}/*!sc*/\n</style>\n<section id="imdb-bmo-footer"><footer class="imdb-footer sc-bcXHqe dvqHbV footer"><div class="footer__app"><div class="ipc-page-content-container ipc-page-content-container--center" role="presentation"><a class="ipc-button ipc-button--double-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-accent1 ipc-button--theme-baseAlt imdb-footer__open-in-app-button" role="button" tabindex="0" aria-disabled="false" href="https://slyb.app.link/SKdyQ6A449"><div class="ipc-button__text">Get the IMDb App</div></a></div></div><div class="footer__sign-in"><div class="ipc-page-content-container ipc-page-content-container--center" role="presentation"><button class="ipc-button ipc-button--double-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-accent1 ipc-button--theme-baseAlt footer__sign-in-button" role="button" tabindex="0" aria-disabled="false" type="button"><div class="ipc-button__text">Sign in for more access</div></button><button class="ipc-button ipc-button--double-padding ipc-button--center-align-content ipc-button--default-height ipc-button--core-baseAlt ipc-button--theme-baseAlt ipc-button--on-accent2 ipc-secondary-button footer__sign-in-mobile-button" role="button" tabindex="0" aria-disabled="false" type="button"><div class="ipc-button__text">Sign in for more access</div></button></div></div><div class="ipc-page-content-container ipc-page-content-container--center footer__links" role="presentation"><div class="imdb-footer__links"><div class="footer__socials"><ul class="ipc-inline-list footer__linksRow baseAlt" role="presentation"><li role="presentation" class="ipc-inline-list__item"><a class="ipc-icon-link ipc-icon-link--baseAlt ipc-icon-link--onBase" title="TikTok" role="button" tabindex="0" aria-label="TikTok" aria-disabled="false" target="_blank" rel="nofollow noopener" href="https://www.tiktok.com/@imdb"><svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--tiktok" id="iconContext-tiktok" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M15.2346 3C15.5316 5.55428 16.9572 7.07713 19.4359 7.23914V10.112C17.9995 10.2524 16.7412 9.78262 15.2778 8.89699V14.2702C15.2778 21.096 7.83633 23.2291 4.84463 18.3365C2.92217 15.1882 4.09941 9.66382 10.2664 9.44241V12.4719C9.7966 12.5475 9.29438 12.6663 8.83536 12.8229C7.46372 13.2873 6.68609 14.1568 6.9021 15.6904C7.31791 18.6281 12.7073 19.4975 12.2591 13.7571V3.0054H15.2346V3Z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-icon-link ipc-icon-link--baseAlt ipc-icon-link--onBase" title="Instagram" role="button" tabindex="0" aria-label="Instagram" aria-disabled="false" target="_blank" rel="nofollow noopener" href="https://instagram.com/imdb"><svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--instagram" id="iconContext-instagram" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M11.997 2.04c-2.715 0-3.056.011-4.122.06-1.064.048-1.79.217-2.426.463a4.901 4.901 0 0 0-1.771 1.151 4.89 4.89 0 0 0-1.153 1.767c-.247.635-.416 1.36-.465 2.422C2.011 8.967 2 9.307 2 12.017s.011 3.049.06 4.113c.049 1.062.218 1.787.465 2.422a4.89 4.89 0 0 0 1.153 1.767 4.901 4.901 0 0 0 1.77 1.15c.636.248 1.363.416 2.427.465 1.066.048 1.407.06 4.122.06s3.055-.012 4.122-.06c1.064-.049 1.79-.217 2.426-.464a4.901 4.901 0 0 0 1.77-1.15 4.89 4.89 0 0 0 1.154-1.768c.247-.635.416-1.36.465-2.422.048-1.064.06-1.404.06-4.113 0-2.71-.012-3.05-.06-4.114-.049-1.062-.218-1.787-.465-2.422a4.89 4.89 0 0 0-1.153-1.767 4.901 4.901 0 0 0-1.77-1.15c-.637-.247-1.363-.416-2.427-.464-1.067-.049-1.407-.06-4.122-.06m0 1.797c2.67 0 2.985.01 4.04.058.974.045 1.503.207 1.856.344.466.181.8.397 1.15.746.349.35.566.682.747 1.147.137.352.3.88.344 1.853.048 1.052.058 1.368.058 4.032 0 2.664-.01 2.98-.058 4.031-.044.973-.207 1.501-.344 1.853a3.09 3.09 0 0 1-.748 1.147c-.35.35-.683.565-1.15.746-.352.137-.88.3-1.856.344-1.054.048-1.37.058-4.04.058-2.669 0-2.985-.01-4.039-.058-.974-.044-1.504-.207-1.856-.344a3.098 3.098 0 0 1-1.15-.746 3.09 3.09 0 0 1-.747-1.147c-.137-.352-.3-.88-.344-1.853-.049-1.052-.059-1.367-.059-4.031 0-2.664.01-2.98.059-4.032.044-.973.207-1.501.344-1.853a3.09 3.09 0 0 1 .748-1.147c.35-.349.682-.565 1.149-.746.352-.137.882-.3 1.856-.344 1.054-.048 1.37-.058 4.04-.058"></path><path d="M11.997 15.342a3.329 3.329 0 0 1-3.332-3.325 3.329 3.329 0 0 1 3.332-3.326 3.329 3.329 0 0 1 3.332 3.326 3.329 3.329 0 0 1-3.332 3.325m0-8.449a5.128 5.128 0 0 0-5.134 5.124 5.128 5.128 0 0 0 5.134 5.123 5.128 5.128 0 0 0 5.133-5.123 5.128 5.128 0 0 0-5.133-5.124m6.536-.203c0 .662-.537 1.198-1.2 1.198a1.198 1.198 0 1 1 1.2-1.197"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-icon-link ipc-icon-link--baseAlt ipc-icon-link--onBase" title="Twitter" role="button" tabindex="0" aria-label="Twitter" aria-disabled="false" target="_blank" rel="nofollow noopener" href="https://twitter.com/imdb"><svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--twitter" id="iconContext-twitter" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M8.29 19.936c7.547 0 11.675-6.13 11.675-11.446 0-.175-.004-.348-.012-.52A8.259 8.259 0 0 0 22 5.886a8.319 8.319 0 0 1-2.356.633 4.052 4.052 0 0 0 1.804-2.225c-.793.46-1.67.796-2.606.976A4.138 4.138 0 0 0 15.847 4c-2.266 0-4.104 1.802-4.104 4.023 0 .315.036.622.107.917a11.728 11.728 0 0 1-8.458-4.203 3.949 3.949 0 0 0-.556 2.022 4 4 0 0 0 1.826 3.348 4.136 4.136 0 0 1-1.858-.503l-.001.051c0 1.949 1.415 3.575 3.292 3.944a4.193 4.193 0 0 1-1.853.07c.522 1.597 2.037 2.76 3.833 2.793a8.34 8.34 0 0 1-5.096 1.722A8.51 8.51 0 0 1 2 18.13a11.785 11.785 0 0 0 6.29 1.807"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-icon-link ipc-icon-link--baseAlt ipc-icon-link--onBase" title="YouTube" role="button" tabindex="0" aria-label="YouTube" aria-disabled="false" target="_blank" rel="nofollow noopener" href="https://youtube.com/imdb/"><svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--youtube" id="iconContext-youtube" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M9.955 14.955v-5.91L15.182 12l-5.227 2.955zm11.627-7.769a2.505 2.505 0 0 0-1.768-1.768C18.254 5 12 5 12 5s-6.254 0-7.814.418c-.86.23-1.538.908-1.768 1.768C2 8.746 2 12 2 12s0 3.254.418 4.814c.23.86.908 1.538 1.768 1.768C5.746 19 12 19 12 19s6.254 0 7.814-.418a2.505 2.505 0 0 0 1.768-1.768C22 15.254 22 12 22 12s0-3.254-.418-4.814z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-icon-link ipc-icon-link--baseAlt ipc-icon-link--onBase" title="Facebook" role="button" tabindex="0" aria-label="Facebook" aria-disabled="false" target="_blank" rel="nofollow noopener" href="https://facebook.com/imdb"><svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--facebook" id="iconContext-facebook" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M20.896 2H3.104C2.494 2 2 2.494 2 3.104v17.792C2 21.506 2.494 22 3.104 22h9.579v-7.745h-2.607v-3.018h2.607V9.01c0-2.584 1.577-3.99 3.882-3.99 1.104 0 2.052.082 2.329.119v2.7h-1.598c-1.254 0-1.496.595-1.496 1.47v1.927h2.989l-.39 3.018h-2.6V22h5.097c.61 0 1.104-.494 1.104-1.104V3.104C22 2.494 21.506 2 20.896 2"></path></svg></a></li></ul></div><div><ul class="ipc-inline-list footer__linksRow baseAlt" role="presentation"><li role="presentation" class="ipc-inline-list__item footerLink--wideScreen"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://slyb.app.link/SKdyQ6A449">Get the IMDb App<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://help.imdb.com/imdb">Help<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://help.imdb.com/article/imdb/general-information/imdb-site-index/GNCX7BHNSPBTFALQ#so">Site Index<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://pro.imdb.com?ref_=cons_tf_pro&amp;rf=cons_tf_pro">IMDbPro<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://www.boxofficemojo.com">Box Office Mojo<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://developer.imdb.com/">IMDb Developer<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li></ul></div><div><ul class="ipc-inline-list footer__linksRow baseAlt" role="presentation"><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" href="https://www.imdb.com/pressroom/?ref_=ft_pr">Press Room</a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://advertising.amazon.com/resources/ad-specs/imdb/">Advertising<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://www.amazon.jobs/en/teams/imdb">Jobs<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" href="/conditions?ref_=ft_cou">Conditions of Use</a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" href="/privacy?ref_=ft_pvc">Privacy Policy</a></li><li role="presentation" class="ipc-inline-list__item"><a class="ipc-link ipc-link--baseAlt ipc-link--touch-target ipc-link--inherit-color" role="button" tabindex="0" aria-disabled="false" target="_blank" href="https://www.amazon.com/b/?&amp;node=5160028011">Interest-Based Ads<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" class="ipc-icon ipc-icon--launch-inline ipc-icon--inline ipc-link__launch-icon" id="iconContext-launch-inline" viewBox="0 0 24 24" fill="currentColor" role="presentation"><path d="M21.6 21.6H2.4V2.4h7.2V0H0v24h24v-9.6h-2.4v7.2zM14.4 0v2.4h4.8L7.195 14.49l2.4 2.4L21.6 4.8v4.8H24V0h-9.6z"></path></svg></a></li><li role="presentation" class="ipc-inline-list__item"><div id="teconsent" class="footerLink--trustarc"></div></li></ul></div></div><div class="imdb-footer__logo footer__logo"><svg aria-label="IMDb, an Amazon company" width="160" height="18" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><title>IMDb, an Amazon company</title><defs><path d="M26.707 2.45c-3.227 2.374-7.906 3.637-11.935 3.637C9.125 6.087 4.04 4.006.193.542-.11.27.161-.101.523.109 4.675 2.517 9.81 3.968 15.111 3.968c3.577 0 7.51-.74 11.127-2.27.546-.23 1.003.358.47.752z" id="ftr__a"></path><path d="M4.113 1.677C3.7 1.15 1.385 1.427.344 1.552c-.315.037-.364-.237-.08-.436C2.112-.178 5.138.196 5.49.629c.354.437-.093 3.462-1.824 4.906-.266.222-.52.104-.401-.19.39-.97 1.261-3.14.848-3.668z" id="ftr__c"></path><path d="M.435 1.805V.548A.311.311 0 0 1 .755.23l5.65-.001c.181 0 .326.13.326.317v1.078c-.002.181-.154.417-.425.791L3.378 6.582c1.087-.026 2.236.137 3.224.69.222.125.282.309.3.49v1.342c0 .185-.203.398-.417.287-1.74-.908-4.047-1.008-5.97.011-.197.104-.403-.107-.403-.292V7.835c0-.204.004-.552.21-.863l3.392-4.85H.761a.314.314 0 0 1-.326-.317z" id="ftr__e"></path><path d="M2.247 9.655H.528a.323.323 0 0 1-.307-.29L.222.569C.222.393.37.253.554.253h1.601a.323.323 0 0 1 .313.295v1.148h.031C2.917.586 3.703.067 4.762.067c1.075 0 1.75.518 2.23 1.629C7.41.586 8.358.067 9.369.067c.722 0 1.508.296 1.99.963.545.74.433 1.813.433 2.757l-.002 5.551a.324.324 0 0 1-.331.317H9.74a.321.321 0 0 1-.308-.316l-.001-4.663c0-.37.032-1.296-.048-1.647-.128-.593-.514-.76-1.011-.76-.418 0-.85.278-1.027.722-.177.445-.161 1.185-.161 1.685v4.662a.323.323 0 0 1-.331.317H5.137a.322.322 0 0 1-.31-.316l-.001-4.663c0-.981.16-2.424-1.059-2.424-1.236 0-1.188 1.406-1.188 2.424v4.662a.324.324 0 0 1-.332.317z" id="ftr__g"></path><path d="M4.037.067c2.551 0 3.931 2.184 3.931 4.96 0 2.684-1.524 4.814-3.931 4.814C1.533 9.84.169 7.656.169 4.935.17 2.195 1.55.067 4.037.067zm.015 1.796c-1.267 0-1.347 1.721-1.347 2.795 0 1.073-.016 3.368 1.332 3.368 1.332 0 1.395-1.851 1.395-2.98 0-.74-.031-1.629-.256-2.332-.193-.61-.578-.851-1.124-.851z" id="ftr__i"></path><path d="M2.206 9.655H.493a.321.321 0 0 1-.308-.316L.182.54a.325.325 0 0 1 .33-.287h1.595c.15.007.274.109.305.245v1.346h.033C2.926.641 3.6.067 4.788.067c.77 0 1.524.277 2.006 1.037.449.703.449 1.887.449 2.739v5.535a.325.325 0 0 1-.33.277H5.19a.324.324 0 0 1-.306-.277V4.602c0-.962.113-2.37-1.075-2.37-.418 0-.803.278-.995.704-.24.537-.273 1.074-.273 1.666v4.736a.328.328 0 0 1-.335.317z" id="ftr__k"></path><path d="M8.314 8.295c.11.156.134.341-.006.455-.35.294-.974.834-1.318 1.139l-.004-.004a.357.357 0 0 1-.406.04c-.571-.473-.673-.692-.986-1.142-.943.958-1.611 1.246-2.834 1.246-1.447 0-2.573-.89-2.573-2.672 0-1.39.756-2.337 1.833-2.8.933-.409 2.235-.483 3.233-.595V3.74c0-.409.032-.89-.209-1.243-.21-.315-.611-.445-.965-.445-.656 0-1.238.335-1.382 1.029-.03.154-.143.307-.298.315l-1.667-.18c-.14-.032-.297-.144-.256-.358C.859.842 2.684.234 4.32.234c.837 0 1.93.222 2.59.853.836.78.755 1.818.755 2.95v2.67c0 .804.335 1.155.65 1.588zM5.253 5.706v-.37c-1.244 0-2.557.265-2.557 1.724 0 .742.386 1.244 1.045 1.244.483 0 .917-.297 1.19-.78.338-.593.322-1.15.322-1.818z" id="ftr__m"></path><path d="M8.203 8.295c.11.156.135.341-.005.455-.352.294-.976.834-1.319 1.139l-.004-.004a.356.356 0 0 1-.406.04c-.571-.473-.673-.692-.985-1.142-.944.958-1.613 1.246-2.835 1.246-1.447 0-2.573-.89-2.573-2.672 0-1.39.756-2.337 1.833-2.8.933-.409 2.236-.483 3.233-.595V3.74c0-.409.032-.89-.21-1.243-.208-.315-.61-.445-.964-.445-.656 0-1.239.335-1.382 1.029-.03.154-.142.307-.298.315l-1.666-.18C.48 3.184.324 3.072.365 2.858.748.842 2.573.234 4.209.234c.836 0 1.93.222 2.59.853.835.78.755 1.818.755 2.95v2.67c0 .804.335 1.155.649 1.588zM5.142 5.706v-.37c-1.243 0-2.557.265-2.557 1.724 0 .742.386 1.244 1.045 1.244.482 0 .917-.297 1.19-.78.338-.593.322-1.15.322-1.818z" id="ftr__o"></path><path d="M2.935 10.148c-.88 0-1.583-.25-2.11-.75-.527-.501-.79-1.171-.79-2.011 0-.902.322-1.622.967-2.159.644-.538 1.511-.806 2.602-.806.694 0 1.475.104 2.342.315V3.513c0-.667-.151-1.136-.455-1.408-.304-.271-.821-.407-1.553-.407-.855 0-1.691.123-2.509.37-.285.087-.464.13-.539.13-.148 0-.223-.111-.223-.334v-.5c0-.16.025-.278.075-.352C.79.938.89.87 1.039.808c.383-.173.87-.312 1.459-.417A9.997 9.997 0 0 1 4.255.234c1.177 0 2.045.244 2.602.731.557.489.836 1.233.836 2.233v6.338c0 .247-.124.37-.372.37h-.798c-.236 0-.373-.117-.41-.351l-.093-.612c-.445.383-.939.68-1.477.89-.54.21-1.076.315-1.608.315zm.446-1.39c.41 0 .836-.08 1.282-.241.447-.16.874-.395 1.283-.704v-1.89a8.408 8.408 0 0 0-1.97-.241c-1.401 0-2.1.537-2.1 1.612 0 .47.13.831.39 1.084.26.254.632.38 1.115.38z" id="ftr__q"></path><path d="M.467 9.907c-.248 0-.372-.124-.372-.37V.883C.095.635.219.51.467.51h.817c.125 0 .22.026.288.075.068.05.115.142.14.277l.111.686C3 .672 4.24.234 5.541.234c.904 0 1.592.238 2.063.713.471.476.707 1.165.707 2.066v6.524c0 .246-.124.37-.372.37H6.842c-.248 0-.372-.124-.372-.37V3.625c0-.655-.133-1.137-.4-1.445-.266-.31-.684-.464-1.254-.464-.979 0-1.94.315-2.881.946v6.875c0 .246-.125.37-.372.37H.467z" id="ftr__s"></path><path d="M4.641 9.859c-1.462 0-2.58-.417-3.355-1.251C.51 7.774.124 6.566.124 4.985c0-1.569.4-2.783 1.2-3.641C2.121.486 3.252.055 4.714.055c.67 0 1.326.118 1.971.353.136.05.232.111.288.185.056.074.083.198.083.37v.501c0 .248-.08.37-.241.37-.062 0-.162-.018-.297-.055a5.488 5.488 0 0 0-1.544-.222c-1.04 0-1.79.262-2.248.787-.459.526-.688 1.362-.688 2.511v.241c0 1.124.232 1.949.697 2.474.465.525 1.198.788 2.203.788a5.98 5.98 0 0 0 1.672-.26c.136-.037.23-.056.279-.056.161 0 .242.124.242.371v.5c0 .162-.025.279-.075.353-.05.074-.148.142-.297.204-.608.259-1.314.389-2.119.389z" id="ftr__u"></path><path d="M4.598 10.185c-1.413 0-2.516-.438-3.31-1.316C.497 7.992.1 6.769.1 5.199c0-1.555.397-2.773 1.19-3.65C2.082.673 3.185.235 4.598.235c1.412 0 2.515.438 3.308 1.316.793.876 1.19 2.094 1.19 3.65 0 1.569-.397 2.792-1.19 3.669-.793.878-1.896 1.316-3.308 1.316zm0-1.483c1.747 0 2.62-1.167 2.62-3.502 0-2.323-.873-3.484-2.62-3.484S1.977 2.877 1.977 5.2c0 2.335.874 3.502 2.62 3.502z" id="ftr__w"></path><path d="M.396 9.907c-.248 0-.371-.124-.371-.37V.883C.025.635.148.51.396.51h.818a.49.49 0 0 1 .288.075c.068.05.115.142.14.277l.111.594C2.943.64 4.102.234 5.23.234c1.152 0 1.934.438 2.342 1.315C8.798.672 10.025.234 11.25.234c.856 0 1.512.24 1.971.722.458.482.688 1.168.688 2.057v6.524c0 .246-.124.37-.372.37h-1.097c-.248 0-.371-.124-.371-.37V3.533c0-.618-.119-1.075-.354-1.372-.235-.297-.607-.445-1.115-.445-.904 0-1.815.278-2.732.834.012.087.018.18.018.278v6.709c0 .246-.124.37-.372.37H6.42c-.249 0-.372-.124-.372-.37V3.533c0-.618-.118-1.075-.353-1.372-.235-.297-.608-.445-1.115-.445-.942 0-1.847.272-2.714.815v7.006c0 .246-.125.37-.372.37H.396z" id="ftr__y"></path><path d="M.617 13.724c-.248 0-.371-.124-.371-.37V.882c0-.247.123-.37.371-.37h.818c.248 0 .39.123.428.37l.093.594C2.897.648 3.944.234 5.096.234c1.203 0 2.15.435 2.845 1.307.693.87 1.04 2.053 1.04 3.548 0 1.52-.365 2.736-1.096 3.65-.731.915-1.704 1.372-2.918 1.372-1.116 0-2.076-.365-2.881-1.094v4.337c0 .246-.125.37-.372.37H.617zM4.54 8.628c1.71 0 2.566-1.149 2.566-3.447 0-1.173-.208-2.044-.624-2.612-.415-.569-1.05-.853-1.904-.853-.88 0-1.711.284-2.491.853v5.17c.805.593 1.623.889 2.453.889z" id="ftr__A"></path><path d="M2.971 10.148c-.88 0-1.583-.25-2.11-.75-.526-.501-.79-1.171-.79-2.011 0-.902.322-1.622.967-2.159.644-.538 1.512-.806 2.602-.806.694 0 1.475.104 2.342.315V3.513c0-.667-.15-1.136-.455-1.408-.304-.271-.821-.407-1.552-.407-.855 0-1.692.123-2.509.37-.285.087-.465.13-.54.13-.148 0-.223-.111-.223-.334v-.5c0-.16.025-.278.075-.352.05-.074.148-.142.297-.204.384-.173.87-.312 1.46-.417A9.991 9.991 0 0 1 4.29.234c1.177 0 2.045.244 2.603.731.557.489.836 1.233.836 2.233v6.338c0 .247-.125.37-.372.37h-.799c-.236 0-.372-.117-.41-.351l-.092-.612a5.09 5.09 0 0 1-1.478.89 4.4 4.4 0 0 1-1.608.315zm.446-1.39c.41 0 .836-.08 1.283-.241.446-.16.874-.395 1.282-.704v-1.89a8.403 8.403 0 0 0-1.97-.241c-1.4 0-2.1.537-2.1 1.612 0 .47.13.831.39 1.084.26.254.632.38 1.115.38z" id="ftr__C"></path><path d="M.503 9.907c-.248 0-.371-.124-.371-.37V.883C.132.635.255.51.503.51h.818a.49.49 0 0 1 .288.075c.068.05.115.142.14.277l.111.686C3.037.672 4.277.234 5.578.234c.904 0 1.592.238 2.063.713.47.476.706 1.165.706 2.066v6.524c0 .246-.123.37-.371.37H6.879c-.248 0-.372-.124-.372-.37V3.625c0-.655-.133-1.137-.4-1.445-.266-.31-.684-.464-1.254-.464-.98 0-1.94.315-2.882.946v6.875c0 .246-.124.37-.371.37H.503z" id="ftr__E"></path><path d="M1.988 13.443c-.397 0-.75-.043-1.059-.13-.15-.037-.251-.1-.307-.185a.684.684 0 0 1-.084-.37v-.483c0-.234.093-.352.28-.352.06 0 .154.013.278.037.124.025.291.037.502.037.459 0 .82-.114 1.087-.343.266-.228.505-.633.716-1.213l.353-.945L.167.675C.08.465.037.316.037.23c0-.149.086-.222.26-.222h1.115c.198 0 .334.03.409.093.075.062.148.197.223.407l2.602 7.19 2.51-7.19c.074-.21.148-.345.222-.407.075-.062.211-.093.41-.093h1.04c.174 0 .261.073.261.222 0 .086-.044.235-.13.445l-4.09 10.377c-.334.853-.725 1.464-1.17 1.835-.446.37-1.017.556-1.711.556z" id="ftr__G"></path></defs><g fill="none" fill-rule="evenodd"><g transform="translate(31.496 11.553)"><mask id="ftr__b" fill="currentColor"><use xlink:href="#ftr__a"></use></mask><path fill="currentColor" mask="url(#ftr__b)" d="M.04 6.088h26.91V.04H.04z"></path></g><g transform="translate(55.433 10.797)"><mask id="ftr__d" fill="currentColor"><use xlink:href="#ftr__c"></use></mask><path fill="currentColor" mask="url(#ftr__d)" d="M.05 5.664h5.564V.222H.05z"></path></g><g transform="translate(55.433 .97)"><mask id="ftr__f" fill="currentColor"><use xlink:href="#ftr__e"></use></mask><path fill="currentColor" mask="url(#ftr__f)" d="M.11 9.444h6.804V.222H.111z"></path></g><g transform="translate(33.008 .97)"><mask id="ftr__h" fill="currentColor"><use xlink:href="#ftr__g"></use></mask><path fill="currentColor" mask="url(#ftr__h)" d="M.191 9.655h11.611V.04H.192z"></path></g><g transform="translate(62.992 .97)"><mask id="ftr__j" fill="currentColor"><use xlink:href="#ftr__i"></use></mask><path fill="currentColor" mask="url(#ftr__j)" d="M.141 9.867h7.831V.04H.142z"></path></g><g transform="translate(72.063 .97)"><mask id="ftr__l" fill="currentColor"><use xlink:href="#ftr__k"></use></mask><path fill="currentColor" mask="url(#ftr__l)" d="M.171 9.655h7.076V.04H.17z"></path></g><g transform="translate(46.11 .718)"><mask id="ftr__n" fill="currentColor"><use xlink:href="#ftr__m"></use></mask><path fill="currentColor" mask="url(#ftr__n)" d="M.181 10.059h8.225V.232H.18z"></path></g><g transform="translate(23.685 .718)"><mask id="ftr__p" fill="currentColor"><use xlink:href="#ftr__o"></use></mask><path fill="currentColor" mask="url(#ftr__p)" d="M.05 10.059h8.255V.232H.05z"></path></g><g transform="translate(0 .718)"><mask id="ftr__r" fill="currentColor"><use xlink:href="#ftr__q"></use></mask><path fill="currentColor" mask="url(#ftr__r)" d="M.03 10.15h7.68V.231H.03z"></path></g><g transform="translate(10.33 .718)"><mask id="ftr__t" fill="currentColor"><use xlink:href="#ftr__s"></use></mask><path fill="currentColor" mask="url(#ftr__t)" d="M.07 9.907h8.255V.232H.071z"></path></g><g transform="translate(84.157 .97)"><mask id="ftr__v" fill="currentColor"><use xlink:href="#ftr__u"></use></mask><path fill="currentColor" mask="url(#ftr__v)" d="M.11 9.867h7.046V.04H.11z"></path></g><g transform="translate(92.472 .718)"><mask id="ftr__x" fill="currentColor"><use xlink:href="#ftr__w"></use></mask><path fill="currentColor" mask="url(#ftr__x)" d="M.08 10.21h9.041V.232H.081z"></path></g><g transform="translate(103.811 .718)"><mask id="ftr__z" fill="currentColor"><use xlink:href="#ftr__y"></use></mask><path fill="currentColor" mask="url(#ftr__z)" d="M.02 9.907H13.93V.232H.02z"></path></g><g transform="translate(120.189 .718)"><mask id="ftr__B" fill="currentColor"><use xlink:href="#ftr__A"></use></mask><path fill="currentColor" mask="url(#ftr__B)" d="M.242 13.747H9.01V.232H.242z"></path></g><g transform="translate(130.772 .718)"><mask id="ftr__D" fill="currentColor"><use xlink:href="#ftr__C"></use></mask><path fill="currentColor" mask="url(#ftr__D)" d="M.06 10.15h7.68V.231H.06z"></path></g><g transform="translate(141.102 .718)"><mask id="ftr__F" fill="currentColor"><use xlink:href="#ftr__E"></use></mask><path fill="currentColor" mask="url(#ftr__F)" d="M.131 9.907h8.224V.232H.131z"></path></g><g transform="translate(150.677 1.222)"><mask id="ftr__H" fill="currentColor"><use xlink:href="#ftr__G"></use></mask><path fill="currentColor" mask="url(#ftr__H)" d="M.02 13.455h9.071V0H.021z"></path></g></g></svg></div><p class="imdb-footer__copyright footer__copyright">\xc2\xa9 1990-<!-- -->2022<!-- --> by IMDb.com, Inc.</p></div></footer><svg style="width:0;height:0;overflow:hidden;display:block" xmlns="http://www.w3.org/2000/svg" version="1.1"><defs><linearGradient id="ipc-svg-gradient-tv-logo-t" x1="31.973%" y1="53.409%" x2="153.413%" y2="-16.853%"><stop stop-color="#D01F49" offset="21.89%"></stop><stop stop-color="#E8138B" offset="83.44%"></stop></linearGradient><linearGradient id="ipc-svg-gradient-tv-logo-v" x1="-38.521%" y1="84.997%" x2="104.155%" y2="14.735%"><stop stop-color="#D01F49" offset="21.89%"></stop><stop stop-color="#E8138B" offset="83.44%"></stop></linearGradient></defs></svg></section>        <script>\n            window.__FOOTER_PROPS__ = {\n                locale: \'en-US\',\n                showDesktopLink: false,\n                desktopLink: undefined,\n                currentUri: \'/search/title/\',\n                showSignIn: true\n            };\n        </script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "desktopFooter", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "desktopFooter", {wb: 1});\n    }\n</script>\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "LoadHeaderJS", {wb: 1});\n    }\n</script>\n\n<script type="text/javascript" src="https://m.media-amazon.com/images/S/sash/JJwdLH4ViTW-kI$.js"></script>\n<script type="text/javascript" src="https://m.media-amazon.com/images/S/sash/LEkTDT9yTAT$m1v.js"></script>\n<script type="text/javascript" src="https://m.media-amazon.com/images/S/sash/a-Sb3G4ONOxtxns.js"></script>\n\n\n<script type="text/javascript" src="https://m.media-amazon.com/images/S/sash/JkadsPuCKXe2k5I.js"></script>\n<script type="text/javascript" src="https://m.media-amazon.com/images/S/sash/rvCWixNlCLN1lmK.js"></script>\n<script type="text/javascript" src="https://m.media-amazon.com/images/S/sash/uYEQ-EZAaZR3vrj.js"></script>\n\n            <script type="text/javascript">\n            function jQueryOnReady(remaining_count) {\n                if (window.jQuery && typeof $.fn.watchlistRibbon !== \'undefined\') {\n                    jQuery(\n                            function() {\n        // Init the watchlist rendering\n        $("a.wlb_watchlist_lite").wlb_lite();\n        $(".lister").watchlistRibbon(".ribbonize");\n\n        // Invoke the rating animation\n        $(".lister-item").rating_animation();\n    }\n\n                    );\n                } else if (remaining_count > 0) {\n                    setTimeout(function() { jQueryOnReady(remaining_count-1) }, 100);\n                }\n            }\n            jQueryOnReady(50);\n            </script>\n\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("bb", "RenderBranchSDK", {wb: 1});\n    }\n</script>\n    <script>\n      if (\'csm\' in window) {\n        csm.measure(\'csm_RenderBranchSDK_started\');\n      }\n    </script>\n    <script class="ics-branch-sdk-script">\n        if (document.domain.startsWith(\'m.\')) {\n\n            function logBranchMetric(metricName, n) {\n                if (window && window.ue && typeof window.ue.count === \'function\') {\n                    window.ue.count(metricName, n);\n                }\n            }\n\n            logBranchMetric(\'BranchSDK.Requests\', 1);\n            if (uet) {\n                uet(\'bb\', \'LoadBranchSDK\', {wb: 1});\n            }\n            (function(b,r,a,n,c,h,_,s,d,k){if(!b[n]||!b[n]._q){for(;s<_.length;)c(h,_[s++]);d=r.createElement(a);d.async=1;d.src="https://cdn.branch.io/branch-2.58.0.min.js";k=r.getElementsByTagName(a)[0];k.parentNode.insertBefore(d,k);b[n]=h}})(window,document,"script","branch",function(b,r){b[r]=function(){b._q.push([r,arguments])}},{_q:[],_v:1},"addListener applyCode autoAppIndex banner closeBanner closeJourney creditHistory credits data deepview deepviewCta first getCode init link logout redeem referrals removeListener sendSMS setBranchViewData setIdentity track validateCode trackCommerceEvent logEvent disableTracking".split(" "), 0);\n            branch.init(\'key_live_jdSfREMXW6WE9FcCt5HWFbhgswmprlIn\', { timeout: 2000 }, (err, data) => {\n                if (uet && uex) {\n                    uet(\'be\', \'LoadBranchSDK\', {wb: 1});\n                    uex(\'ld\', \'LoadBranchSDK\', {wb: 1} )\n                }\n                if (err) {\n                    console.log(\'Branch init error\', err);\n                    logBranchMetric(\'BranchSDK.Error\', 1);\n                } else {\n                    logBranchMetric(\'BranchSDK.Error\', 0);\n                    logBranchMetric(\'BranchSDK.Initialized\', 1);\n\n                    branch.setBranchViewData(\n                        {\n                            data: {\n                                \'$deeplink_path\': \'\\/search/title/\'\n                            }\n                        }\n                    );\n                    branch.setIdentity(\'000-0000000-0000000\', (err, data) => {\n                        if (err) {\n                            console.log(\'Branch setIdentity error\', err);\n                            logBranchMetric(\'BranchSDK.Error\', 1);\n                        } else {\n                            logBranchMetric(\'BranchSDK.Error\', 0);\n                        }\n                    });\n                }\n            });\n        }\n    </script>\n    <script>\n      if (\'csm\' in window) {\n        csm.measure(\'csm_RenderBranchSDK_finished\');\n      }\n    </script>\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "RenderBranchSDK", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "RenderBranchSDK", {wb: 1});\n    }\n</script>\n\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("be", "LoadFooterJS", {wb: 1});\n    }\n</script>\n<script>\n    if (typeof uex == \'function\') {\n      uex("ld", "LoadFooterJS", {wb: 1});\n    }\n</script>\n        \n        <div id="servertime" time="203"/>\n\n\n\n<script>\n    if (typeof uet == \'function\') {\n      uet("be");\n    }\n</script>\n        \n    </body>\n</html>\n'


### Test  process_str_with_comma 


```python
def test_process_str_with_comma():
    """output:
    string: it is a string
    string: "it is a string, right"
    """
    string = 'it is a string'
    print ("string: " + process_str_with_comma(string))
    string = 'it is a string, right'
    print ("string: " + process_str_with_comma(string))

```


```python
test_process_str_with_comma()
```

    string: it is a string
    string: "it is a string, right"


***

## read_m_from_url

Inside this function, you need to write your code to pull the movies information.
For each movie, you need to pull :
- movie_id
- title 
- rank
- year
- runtime
- rating
- votes 
- genres

To give examples on how to pull data from web bage, I have included the code to pull the movie_id.
You need to inculde your code to pull the other needed movie information (title, rank, year, ......). You should have no missing values for each of the collected data.


```python
def read_m_from_url(url, num_of_m=50):
    print(url)
    
    html_string = read_html(url)
    soup = BeautifulSoup(html_string, "html.parser")

    
    movie_list = soup.find('div', 'lister-list')
    
    list_movies = [] 
    
    count = 0
    

    divs=  movie_list.find_all('div','lister-item mode-advanced')
    for d in divs:
        dict_each_movie = {}
        
        # movie_id
        try:
            h = d.find('h3','lister-item-header') 
            movie_id= h.find('a').attrs['href']
            movie_id= movie_id[7:-1]
            
        except:
            movie_id=""
        finally:
            dict_each_movie["movie_id"] = movie_id
            print(movie_id)
            
        # title
        try:
            m = d.find('h3', 'lister-item-header')
            title = m.find('a')
            title = title.get_text()
        except:
            title = ""
        finally: 
            dict_each_movie["title"] = title
            print(title)

        # rank
        try:
            m = d.find('h3', 'lister-item-header')
            rank = m.find('span', 'lister-item-index unbold text-primary' )
            rank = rank.get_text()
        except:
            rank = ""
        finally: 
            dict_each_movie["rank"] = rank
            print(rank)    

        # year 
        try:
            m = d.find('h3', 'lister-item-header')
            year = m.find('span', 'lister-item-year text-muted unbold')
            year = year.get_text().replace('(', "").replace(')', "").replace('I',"").replace(" ", "")

        except:
            year = ""
        finally: 
            dict_each_movie["year"] = year
            print(year) 

        # runtime
        try:
            m = d.find('p', 'text-muted')
            rtime = m.find('span', 'runtime')
            rtime = rtime.get_text()

        except:
            rtime = ""
        finally: 
            dict_each_movie["runtime"] = rtime
            print(rtime) 

        # rating 
        try:
            m = d.find('div', 'ratings-bar')
            rating = m.find('strong')
            rating = rating.get_text()
        except:
            rating = ""
        finally: 
            dict_each_movie["rating"] = rating
            print(rating)   

        # votes
        try: 
            m = d.find('p', 'sort-num_votes-visible')
            votes = m.find_next('span').find_next('span')
            votes = votes.get_text()
        except:
            votes = ""
        finally: 
            dict_each_movie["votes"] = votes
            print(votes)   

        # genres
        try:
            m = d.find('p', 'text-muted')
            genres = m.find('span', 'genre')
            genres = genres.get_text().replace("\n", "")

        except:
            genres = ""
        finally: 
            dict_each_movie["genres"] = genres
            print(genres)
            
        list_movies.append(dict_each_movie) 

        count +=1
        print('===============================')
        print()
        if count == num_of_m:
            break 

    return list_movies

```

**Comments on pulling movie information**: For each variable scraped from the IMDb url, I first inspected the HTML code to find the relevant code block. Using the '.find' function, I filtered through the HTML code to retreive the relevant data. This filtered data was stored in the 'dict_each_movie' dictionary. The '.append' function added each variable to the 'list_movies' dataframe.

**Notes**: 

- Since data for 'year' was in the format '(20XX)', the data was modified to remove '( )' before it was stored in the dictionary. The 'year' entry for the film 'Joker' had an additional 'I' before it, which was also removed before being stored in the dictionary. <br><br>

- The 'find.next' function was used to pull observations for 'votes' since the number value was the second 'span' element under 'sort-num_votes-visible'. <br><br>

- After running the 'test_read_m_from_url' function, I observed that an unnecessary line break was insterted before 'genre' observations in the output. To correct this, I removed '\n' from 'genre' observations before they were added to the dictionary. 

### Test  read_m_from_url


```python
def test_read_m_from_url():
    """ output:
    Movies list: [{'movie_id': 'tt7286456', 'title': 'Joker', 'year': '(2019)', 'rank': '1.', 'runtime': '122 min', 'rating': '8.4', 'votes': '1,074,230'}, {'movie_id': 'tt4154796', 'title': 'Avengers: Endgame', 'year': '(2019)', 'rank': '2.', 'runtime': '181 min', 'rating': '8.4', 'votes': '945,461'}, {'movie_id': 'tt4154756', 'title': 'Avengers: Infinity War', 'year': '(2018)', 'rank': '3.', 'runtime': '149 min', 'rating': '8.4', 'votes': '928,596'}, {'movie_id': 'tt1825683', 'title': 'Black Panther', 'year': '(2018)', 'rank': '4.', 'runtime': '134 min', 'rating': '7.3', 'votes': '678,964'}, {'movie_id': 'tt6751668', 'title': 'Parasite', 'year': '(2019)', 'rank': '5.', 'runtime': '132 min', 'rating': '8.6', 'votes': '666,646'}, {'movie_id': 'tt7131622', 'title': 'Once Upon a Time... In Hollywood', 'year': '(2019)', 'rank': '6.', 'runtime': '161 min', 'rating': '7.6', 'votes': '642,048'}, {'movie_id': 'tt8946378', 'title': 'Knives Out', 'year': '(2019)', 'rank': '7.', 'runtime': '130 min', 'rating': '7.9', 'votes': '534,299'}, {'movie_id': 'tt5463162', 'title': 'Deadpool 2', 'year': '(2018)', 'rank': '8.', 'runtime': '119 min', 'rating': '7.7', 'votes': '519,760'}, {'movie_id': 'tt8579674', 'title': '1917', 'year': '(2019)', 'rank': '9.', 'runtime': '119 min', 'rating': '8.3', 'votes': '495,380'}, {'movie_id': 'tt4154664', 'title': 'Captain Marvel', 'year': '(2019)', 'rank': '10.', 'runtime': '123 min', 'rating': '6.8', 'votes': '493,817'}, {'movie_id': 'tt1727824', 'title': 'Bohemian Rhapsody', 'year': '(2018)', 'rank': '11.', 'runtime': '134 min', 'rating': '7.9', 'votes': '489,064'}, {'movie_id': 'tt6644200', 'title': 'A Quiet Place', 'year': '(2018)', 'rank': '12.', 'runtime': '90 min', 'rating': '7.5', 'votes': '482,141'}, {'movie_id': 'tt4633694', 'title': 'Spider-Man: Into the Spider-Verse', 'year': '(2018)', 'rank': '13.', 'runtime': '117 min', 'rating': '8.4', 'votes': '430,153'}, {'movie_id': 'tt6966692', 'title': 'Green Book', 'year': '(2018)', 'rank': '14.', 'runtime': '130 min', 'rating': '8.2', 'votes': '428,762'}, {'movie_id': 'tt6723592', 'title': 'Tenet', 'year': '(2020)', 'rank': '15.', 'runtime': '150 min', 'rating': '7.4', 'votes': '426,125'}, {'movie_id': 'tt1477834', 'title': 'Aquaman', 'year': '(2018)', 'rank': '16.', 'runtime': '143 min', 'rating': '6.9', 'votes': '417,286'}, {'movie_id': 'tt1270797', 'title': 'Venom', 'year': '(2018)', 'rank': '17.', 'runtime': '112 min', 'rating': '6.7', 'votes': '410,565'}, {'movie_id': 'tt2527338', 'title': 'Star Wars: The Rise Of Skywalker', 'year': '(2019)', 'rank': '18.', 'runtime': '141 min', 'rating': '6.5', 'votes': '404,527'}, {'movie_id': 'tt1677720', 'title': 'Ready Player One', 'year': '(2018)', 'rank': '19.', 'runtime': '140 min', 'rating': '7.4', 'votes': '398,599'}, {'movie_id': 'tt6320628', 'title': 'Spider-Man: Far from Home', 'year': '(2019)', 'rank': '20.', 'runtime': '129 min', 'rating': '7.4', 'votes': '383,087'}, {'movie_id': 'tt1517451', 'title': 'A Star Is Born', 'year': '(2018)', 'rank': '21.', 'runtime': '136 min', 'rating': '7.6', 'votes': '356,745'}, {'movie_id': 'tt1302006', 'title': 'The Irishman', 'year': '(2019)', 'rank': '22.', 'runtime': '209 min', 'rating': '7.8', 'votes': '352,691'}, {'movie_id': 'tt5095030', 'title': 'Ant-Man and the Wasp', 'year': '(2018)', 'rank': '23.', 'runtime': '118 min', 'rating': '7.0', 'votes': '346,822'}, {'movie_id': 'tt2584384', 'title': 'Jojo Rabbit', 'year': '(2019)', 'rank': '24.', 'runtime': '108 min', 'rating': '7.9', 'votes': '340,733'}, {'movie_id': 'tt1950186', 'title': 'Ford v Ferrari', 'year': '(2019)', 'rank': '25.', 'runtime': '152 min', 'rating': '8.1', 'votes': '337,490'}, {'movie_id': 'tt3778644', 'title': 'Solo: A Star Wars Story', 'year': '(2018)', 'rank': '26.', 'runtime': '135 min', 'rating': '6.9', 'votes': '313,683'}, {'movie_id': 'tt4912910', 'title': 'Mission: Impossible - Fallout', 'year': '(2018)', 'rank': '27.', 'runtime': '147 min', 'rating': '7.7', 'votes': '308,761'}, {'movie_id': 'tt6146586', 'title': 'John Wick: Chapter 3 - Parabellum', 'year': '(2019)', 'rank': '28.', 'runtime': '130 min', 'rating': '7.4', 'votes': '306,871'}, {'movie_id': 'tt2737304', 'title': 'Bird Box', 'year': '(2018)', 'rank': '29.', 'runtime': '124 min', 'rating': '6.6', 'votes': '305,135'}, {'movie_id': 'tt2798920', 'title': 'Annihilation', 'year': '(I) (2018)', 'rank': '30.', 'runtime': '115 min', 'rating': '6.8', 'votes': '298,808'}, {'movie_id': 'tt0448115', 'title': 'Shazam!', 'year': '(2019)', 'rank': '31.', 'runtime': '132 min', 'rating': '7.0', 'votes': '295,848'}, {'movie_id': 'tt4881806', 'title': 'Jurassic World: Fallen Kingdom', 'year': '(2018)', 'rank': '32.', 'runtime': '128 min', 'rating': '6.2', 'votes': '283,862'}, {'movie_id': 'tt8367814', 'title': 'The Gentlemen', 'year': '(2019)', 'rank': '33.', 'runtime': '113 min', 'rating': '7.8', 'votes': '282,900'}, {'movie_id': 'tt2948372', 'title': 'Soul', 'year': '(2020)', 'rank': '34.', 'runtime': '100 min', 'rating': '8.1', 'votes': '280,267'}, {'movie_id': 'tt7653254', 'title': 'Marriage Story', 'year': '(2019)', 'rank': '35.', 'runtime': '137 min', 'rating': '7.9', 'votes': '274,033'}, {'movie_id': 'tt7784604', 'title': 'Hereditary', 'year': '(2018)', 'rank': '36.', 'runtime': '127 min', 'rating': '7.3', 'votes': '270,157'}, {'movie_id': 'tt3606756', 'title': 'Incredibles 2', 'year': '(2018)', 'rank': '37.', 'runtime': '118 min', 'rating': '7.6', 'votes': '269,810'}, {'movie_id': 'tt6857112', 'title': 'Us', 'year': '(II) (2019)', 'rank': '38.', 'runtime': '116 min', 'rating': '6.8', 'votes': '257,673'}, {'movie_id': 'tt8772262', 'title': 'Midsommar', 'year': '(2019)', 'rank': '39.', 'runtime': '148 min', 'rating': '7.1', 'votes': '255,303'}, {'movie_id': 'tt0437086', 'title': 'Alita: Battle Angel', 'year': '(2019)', 'rank': '40.', 'runtime': '122 min', 'rating': '7.3', 'votes': '247,522'}, {'movie_id': 'tt5727208', 'title': 'Uncut Gems', 'year': '(2019)', 'rank': '41.', 'runtime': '135 min', 'rating': '7.4', 'votes': '246,741'}, {'movie_id': 'tt6139732', 'title': 'Aladdin', 'year': '(2019)', 'rank': '42.', 'runtime': '128 min', 'rating': '6.9', 'votes': '245,331'}, {'movie_id': 'tt7349662', 'title': 'BlacKkKlansman', 'year': '(2018)', 'rank': '43.', 'runtime': '135 min', 'rating': '7.5', 'votes': '242,325'}, {'movie_id': 'tt4123430', 'title': 'Fantastic Beasts: The Crimes of Grindelwald', 'year': '(2018)', 'rank': '44.', 'runtime': '134 min', 'rating': '6.5', 'votes': '239,448'}, {'movie_id': 'tt7126948', 'title': 'Wonder Woman 1984', 'year': '(2020)', 'rank': '45.', 'runtime': '151 min', 'rating': '5.4', 'votes': '232,962'}, {'movie_id': 'tt7349950', 'title': 'It Chapter Two', 'year': '(2019)', 'rank': '46.', 'runtime': '169 min', 'rating': '6.5', 'votes': '230,874'}, {'movie_id': 'tt6105098', 'title': 'The Lion King', 'year': '(2019)', 'rank': '47.', 'runtime': '118 min', 'rating': '6.8', 'votes': '226,997'}, {'movie_id': 'tt6823368', 'title': 'Glass', 'year': '(2019)', 'rank': '48.', 'runtime': '129 min', 'rating': '6.6', 'votes': '224,677'}, {'movie_id': 'tt1979376', 'title': 'Toy Story 4', 'year': '(2019)', 'rank': '49.', 'runtime': '100 min', 'rating': '7.7', 'votes': '223,650'}, {'movie_id': 'tt2704998', 'title': 'Game Night', 'year': '(I) (2018)', 'rank': '50.', 'runtime': '100 min', 'rating': '6.9', 'votes': '220,159'}]    
    """
    url = "http://www.imdb.com/search/title?at=0&sort=num_votes,desc&start=1&title_type=feature&year=2018,2020"
    print ("Movies list: ", read_m_from_url(url))
```


```python
 test_read_m_from_url()
```

    http://www.imdb.com/search/title?at=0&sort=num_votes,desc&start=1&title_type=feature&year=2018,2020
    tt7286456
    Joker
    1.
    2019
    122 min
    8.4
    1,271,181
    Crime, Drama, Thriller            
    ===============================
    
    tt4154796
    Avengers: Endgame
    2.
    2019
    181 min
    8.4
    1,121,459
    Action, Adventure, Drama            
    ===============================
    
    tt4154756
    Avengers: Infinity War
    3.
    2018
    149 min
    8.4
    1,073,326
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt6751668
    Parasite
    4.
    2019
    132 min
    8.5
    794,414
    Drama, Thriller            
    ===============================
    
    tt1825683
    Black Panther
    5.
    2018
    134 min
    7.3
    767,098
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt7131622
    Once Upon a Time in Hollywood
    6.
    2019
    161 min
    7.6
    731,841
    Comedy, Drama            
    ===============================
    
    tt8946378
    Knives Out
    7.
    2019
    130 min
    7.9
    625,540
    Comedy, Crime, Drama            
    ===============================
    
    tt8579674
    1917
    8.
    2019
    119 min
    8.2
    585,340
    Action, Drama, War            
    ===============================
    
    tt5463162
    Deadpool 2
    9.
    2018
    119 min
    7.7
    581,115
    Action, Adventure, Comedy            
    ===============================
    
    tt4154664
    Captain Marvel
    10.
    2019
    123 min
    6.8
    559,308
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt1727824
    Bohemian Rhapsody
    11.
    2018
    134 min
    7.9
    537,886
    Biography, Drama, Music            
    ===============================
    
    tt6644200
    A Quiet Place
    12.
    2018
    90 min
    7.5
    528,078
    Drama, Horror, Sci-Fi            
    ===============================
    
    tt4633694
    Spider-Man: Into the Spider-Verse
    13.
    2018
    117 min
    8.4
    524,990
    Animation, Action, Adventure            
    ===============================
    
    tt6723592
    Tenet
    14.
    2020
    150 min
    7.3
    501,142
    Action, Sci-Fi, Thriller            
    ===============================
    
    tt6320628
    Spider-Man: Far from Home
    15.
    2019
    129 min
    7.4
    490,791
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt6966692
    Green Book
    16.
    2018
    130 min
    8.2
    489,304
    Biography, Comedy, Drama            
    ===============================
    
    tt1270797
    Venom
    17.
    2018
    112 min
    6.6
    484,803
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt1477834
    Aquaman
    18.
    2018
    143 min
    6.8
    474,104
    Action, Adventure, Fantasy            
    ===============================
    
    tt2527338
    Star Wars: The Rise Of Skywalker
    19.
    2019
    141 min
    6.5
    450,328
    Action, Adventure, Fantasy            
    ===============================
    
    tt1677720
    Ready Player One
    20.
    2018
    140 min
    7.4
    436,280
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt5095030
    Ant-Man and the Wasp
    21.
    2018
    118 min
    7.0
    401,934
    Action, Adventure, Comedy            
    ===============================
    
    tt1950186
    Ford v Ferrari
    22.
    2019
    152 min
    8.1
    392,046
    Action, Biography, Drama            
    ===============================
    
    tt2584384
    Jojo Rabbit
    23.
    2019
    108 min
    7.9
    388,254
    Comedy, Drama, War            
    ===============================
    
    tt1302006
    The Irishman
    24.
    2019
    209 min
    7.8
    387,450
    Biography, Crime, Drama            
    ===============================
    
    tt1517451
    A Star Is Born
    25.
    2018
    136 min
    7.6
    384,165
    Drama, Music, Romance            
    ===============================
    
    tt3778644
    Solo: A Star Wars Story
    26.
    2018
    135 min
    6.9
    347,374
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt6146586
    John Wick: Chapter 3 - Parabellum
    27.
    2019
    130 min
    7.4
    343,721
    Action, Crime, Thriller            
    ===============================
    
    tt2737304
    Bird Box
    28.
    2018
    124 min
    6.6
    342,273
    Horror, Mystery, Sci-Fi            
    ===============================
    
    tt8367814
    The Gentlemen
    29.
    2019
    113 min
    7.8
    338,310
    Action, Comedy, Crime            
    ===============================
    
    tt0448115
    Shazam!
    30.
    2019
    132 min
    7.0
    338,065
    Action, Adventure, Comedy            
    ===============================
    
    tt4912910
    Mission: Impossible - Fallout
    31.
    2018
    147 min
    7.7
    333,151
    Action, Adventure, Thriller            
    ===============================
    
    tt2948372
    Soul
    32.
    2020
    100 min
    8.0
    328,563
    Animation, Adventure, Comedy            
    ===============================
    
    tt2798920
    Annihilation
    33.
    2018
    115 min
    6.8
    327,214
    Adventure, Drama, Horror            
    ===============================
    
    tt8772262
    Midsommar
    34.
    2019
    148 min
    7.1
    321,719
    Drama, Horror, Mystery            
    ===============================
    
    tt4881806
    Jurassic World: Fallen Kingdom
    35.
    2018
    128 min
    6.1
    317,221
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt7784604
    Hereditary
    36.
    2018
    127 min
    7.3
    316,203
    Drama, Horror, Mystery            
    ===============================
    
    tt7653254
    Marriage Story
    37.
    2019
    137 min
    7.9
    309,090
    Drama, Romance            
    ===============================
    
    tt6857112
    Us
    38.
    2019
    116 min
    6.8
    299,452
    Horror, Mystery, Thriller            
    ===============================
    
    tt3606756
    Incredibles 2
    39.
    2018
    118 min
    7.6
    297,461
    Animation, Action, Adventure            
    ===============================
    
    tt4123430
    Fantastic Beasts: The Crimes of Grindelwald
    40.
    2018
    134 min
    6.5
    283,597
    Adventure, Family, Fantasy            
    ===============================
    
    tt5727208
    Uncut Gems
    41.
    2019
    135 min
    7.4
    282,052
    Crime, Drama, Thriller            
    ===============================
    
    tt0437086
    Alita: Battle Angel
    42.
    2019
    122 min
    7.3
    268,520
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt7126948
    Wonder Woman 1984
    43.
    2020
    151 min
    5.4
    267,271
    Action, Adventure, Fantasy            
    ===============================
    
    tt7349950
    It Chapter Two
    44.
    2019
    169 min
    6.5
    266,323
    Drama, Fantasy, Horror            
    ===============================
    
    tt6139732
    Aladdin
    45.
    2019
    128 min
    6.9
    266,237
    Adventure, Comedy, Family            
    ===============================
    
    tt7349662
    BlacKkKlansman
    46.
    2018
    135 min
    7.5
    264,598
    Biography, Comedy, Crime            
    ===============================
    
    tt1979376
    Toy Story 4
    47.
    2019
    100 min
    7.7
    250,943
    Animation, Adventure, Comedy            
    ===============================
    
    tt7975244
    Jumanji: The Next Level
    48.
    2019
    123 min
    6.7
    250,937
    Action, Adventure, Comedy            
    ===============================
    
    tt9243946
    El Camino: A Breaking Bad Movie
    49.
    2019
    122 min
    7.3
    250,282
    Action, Crime, Drama            
    ===============================
    
    tt6105098
    The Lion King
    50.
    2019
    118 min
    6.8
    245,589
    Animation, Adventure, Drama            
    ===============================
    
    Movies list:  [{'movie_id': 'tt7286456', 'title': 'Joker', 'rank': '1.', 'year': '2019', 'runtime': '122 min', 'rating': '8.4', 'votes': '1,271,181', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt4154796', 'title': 'Avengers: Endgame', 'rank': '2.', 'year': '2019', 'runtime': '181 min', 'rating': '8.4', 'votes': '1,121,459', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt4154756', 'title': 'Avengers: Infinity War', 'rank': '3.', 'year': '2018', 'runtime': '149 min', 'rating': '8.4', 'votes': '1,073,326', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt6751668', 'title': 'Parasite', 'rank': '4.', 'year': '2019', 'runtime': '132 min', 'rating': '8.5', 'votes': '794,414', 'genres': 'Drama, Thriller            '}, {'movie_id': 'tt1825683', 'title': 'Black Panther', 'rank': '5.', 'year': '2018', 'runtime': '134 min', 'rating': '7.3', 'votes': '767,098', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt7131622', 'title': 'Once Upon a Time in Hollywood', 'rank': '6.', 'year': '2019', 'runtime': '161 min', 'rating': '7.6', 'votes': '731,841', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt8946378', 'title': 'Knives Out', 'rank': '7.', 'year': '2019', 'runtime': '130 min', 'rating': '7.9', 'votes': '625,540', 'genres': 'Comedy, Crime, Drama            '}, {'movie_id': 'tt8579674', 'title': '1917', 'rank': '8.', 'year': '2019', 'runtime': '119 min', 'rating': '8.2', 'votes': '585,340', 'genres': 'Action, Drama, War            '}, {'movie_id': 'tt5463162', 'title': 'Deadpool 2', 'rank': '9.', 'year': '2018', 'runtime': '119 min', 'rating': '7.7', 'votes': '581,115', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt4154664', 'title': 'Captain Marvel', 'rank': '10.', 'year': '2019', 'runtime': '123 min', 'rating': '6.8', 'votes': '559,308', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt1727824', 'title': 'Bohemian Rhapsody', 'rank': '11.', 'year': '2018', 'runtime': '134 min', 'rating': '7.9', 'votes': '537,886', 'genres': 'Biography, Drama, Music            '}, {'movie_id': 'tt6644200', 'title': 'A Quiet Place', 'rank': '12.', 'year': '2018', 'runtime': '90 min', 'rating': '7.5', 'votes': '528,078', 'genres': 'Drama, Horror, Sci-Fi            '}, {'movie_id': 'tt4633694', 'title': 'Spider-Man: Into the Spider-Verse', 'rank': '13.', 'year': '2018', 'runtime': '117 min', 'rating': '8.4', 'votes': '524,990', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt6723592', 'title': 'Tenet', 'rank': '14.', 'year': '2020', 'runtime': '150 min', 'rating': '7.3', 'votes': '501,142', 'genres': 'Action, Sci-Fi, Thriller            '}, {'movie_id': 'tt6320628', 'title': 'Spider-Man: Far from Home', 'rank': '15.', 'year': '2019', 'runtime': '129 min', 'rating': '7.4', 'votes': '490,791', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt6966692', 'title': 'Green Book', 'rank': '16.', 'year': '2018', 'runtime': '130 min', 'rating': '8.2', 'votes': '489,304', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt1270797', 'title': 'Venom', 'rank': '17.', 'year': '2018', 'runtime': '112 min', 'rating': '6.6', 'votes': '484,803', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt1477834', 'title': 'Aquaman', 'rank': '18.', 'year': '2018', 'runtime': '143 min', 'rating': '6.8', 'votes': '474,104', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt2527338', 'title': 'Star Wars: The Rise Of Skywalker', 'rank': '19.', 'year': '2019', 'runtime': '141 min', 'rating': '6.5', 'votes': '450,328', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt1677720', 'title': 'Ready Player One', 'rank': '20.', 'year': '2018', 'runtime': '140 min', 'rating': '7.4', 'votes': '436,280', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt5095030', 'title': 'Ant-Man and the Wasp', 'rank': '21.', 'year': '2018', 'runtime': '118 min', 'rating': '7.0', 'votes': '401,934', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt1950186', 'title': 'Ford v Ferrari', 'rank': '22.', 'year': '2019', 'runtime': '152 min', 'rating': '8.1', 'votes': '392,046', 'genres': 'Action, Biography, Drama            '}, {'movie_id': 'tt2584384', 'title': 'Jojo Rabbit', 'rank': '23.', 'year': '2019', 'runtime': '108 min', 'rating': '7.9', 'votes': '388,254', 'genres': 'Comedy, Drama, War            '}, {'movie_id': 'tt1302006', 'title': 'The Irishman', 'rank': '24.', 'year': '2019', 'runtime': '209 min', 'rating': '7.8', 'votes': '387,450', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt1517451', 'title': 'A Star Is Born', 'rank': '25.', 'year': '2018', 'runtime': '136 min', 'rating': '7.6', 'votes': '384,165', 'genres': 'Drama, Music, Romance            '}, {'movie_id': 'tt3778644', 'title': 'Solo: A Star Wars Story', 'rank': '26.', 'year': '2018', 'runtime': '135 min', 'rating': '6.9', 'votes': '347,374', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt6146586', 'title': 'John Wick: Chapter 3 - Parabellum', 'rank': '27.', 'year': '2019', 'runtime': '130 min', 'rating': '7.4', 'votes': '343,721', 'genres': 'Action, Crime, Thriller            '}, {'movie_id': 'tt2737304', 'title': 'Bird Box', 'rank': '28.', 'year': '2018', 'runtime': '124 min', 'rating': '6.6', 'votes': '342,273', 'genres': 'Horror, Mystery, Sci-Fi            '}, {'movie_id': 'tt8367814', 'title': 'The Gentlemen', 'rank': '29.', 'year': '2019', 'runtime': '113 min', 'rating': '7.8', 'votes': '338,310', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt0448115', 'title': 'Shazam!', 'rank': '30.', 'year': '2019', 'runtime': '132 min', 'rating': '7.0', 'votes': '338,065', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt4912910', 'title': 'Mission: Impossible - Fallout', 'rank': '31.', 'year': '2018', 'runtime': '147 min', 'rating': '7.7', 'votes': '333,151', 'genres': 'Action, Adventure, Thriller            '}, {'movie_id': 'tt2948372', 'title': 'Soul', 'rank': '32.', 'year': '2020', 'runtime': '100 min', 'rating': '8.0', 'votes': '328,563', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt2798920', 'title': 'Annihilation', 'rank': '33.', 'year': '2018', 'runtime': '115 min', 'rating': '6.8', 'votes': '327,214', 'genres': 'Adventure, Drama, Horror            '}, {'movie_id': 'tt8772262', 'title': 'Midsommar', 'rank': '34.', 'year': '2019', 'runtime': '148 min', 'rating': '7.1', 'votes': '321,719', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt4881806', 'title': 'Jurassic World: Fallen Kingdom', 'rank': '35.', 'year': '2018', 'runtime': '128 min', 'rating': '6.1', 'votes': '317,221', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt7784604', 'title': 'Hereditary', 'rank': '36.', 'year': '2018', 'runtime': '127 min', 'rating': '7.3', 'votes': '316,203', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt7653254', 'title': 'Marriage Story', 'rank': '37.', 'year': '2019', 'runtime': '137 min', 'rating': '7.9', 'votes': '309,090', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt6857112', 'title': 'Us', 'rank': '38.', 'year': '2019', 'runtime': '116 min', 'rating': '6.8', 'votes': '299,452', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt3606756', 'title': 'Incredibles 2', 'rank': '39.', 'year': '2018', 'runtime': '118 min', 'rating': '7.6', 'votes': '297,461', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt4123430', 'title': 'Fantastic Beasts: The Crimes of Grindelwald', 'rank': '40.', 'year': '2018', 'runtime': '134 min', 'rating': '6.5', 'votes': '283,597', 'genres': 'Adventure, Family, Fantasy            '}, {'movie_id': 'tt5727208', 'title': 'Uncut Gems', 'rank': '41.', 'year': '2019', 'runtime': '135 min', 'rating': '7.4', 'votes': '282,052', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt0437086', 'title': 'Alita: Battle Angel', 'rank': '42.', 'year': '2019', 'runtime': '122 min', 'rating': '7.3', 'votes': '268,520', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt7126948', 'title': 'Wonder Woman 1984', 'rank': '43.', 'year': '2020', 'runtime': '151 min', 'rating': '5.4', 'votes': '267,271', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt7349950', 'title': 'It Chapter Two', 'rank': '44.', 'year': '2019', 'runtime': '169 min', 'rating': '6.5', 'votes': '266,323', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt6139732', 'title': 'Aladdin', 'rank': '45.', 'year': '2019', 'runtime': '128 min', 'rating': '6.9', 'votes': '266,237', 'genres': 'Adventure, Comedy, Family            '}, {'movie_id': 'tt7349662', 'title': 'BlacKkKlansman', 'rank': '46.', 'year': '2018', 'runtime': '135 min', 'rating': '7.5', 'votes': '264,598', 'genres': 'Biography, Comedy, Crime            '}, {'movie_id': 'tt1979376', 'title': 'Toy Story 4', 'rank': '47.', 'year': '2019', 'runtime': '100 min', 'rating': '7.7', 'votes': '250,943', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt7975244', 'title': 'Jumanji: The Next Level', 'rank': '48.', 'year': '2019', 'runtime': '123 min', 'rating': '6.7', 'votes': '250,937', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt9243946', 'title': 'El Camino: A Breaking Bad Movie', 'rank': '49.', 'year': '2019', 'runtime': '122 min', 'rating': '7.3', 'votes': '250,282', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt6105098', 'title': 'The Lion King', 'rank': '50.', 'year': '2019', 'runtime': '118 min', 'rating': '6.8', 'votes': '245,589', 'genres': 'Animation, Adventure, Drama            '}]


We can see that 50 entries from the first page of the IMDb list were successfully scraped using the 'test_read_from_url' function. 

##  read_m_by_voting


```python
m_per_page = 50 # by default, imdb return 50 movies page url.
def read_m_by_voting(first_year, last_year, top_number):
    
    current_index = 1  # initialize current_index. In the first iteration, we need to have start = 1.
    
    final_list = []  # initialize the return value. This method returns a list. Each item in the list is a dictionary. 
                     # Each dictionary includes information regarding a movie.

    for i in range(int(math.ceil(top_number/50.0))):
        url= 'http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start='+str(current_index)+'&title_type=feature&year='+str(first_year)+','+ str(last_year)

        if (i+1) == len(range( int(math.ceil(top_number/50.0)))):
            lis = read_m_from_url(url, top_number - current_index + 1)
        else:
            lis = read_m_from_url(url, m_per_page)
        final_list += lis
        current_index +=50

    return final_list
```

### Test read_m_by_voting


```python
def test_read_m_by_voting():
    """output:
    [{'movie_id': 'tt7286456', 'title': 'Joker', 'year': '(2019)', 'rank': '1.', 'genres': 'Crime, Drama, Thriller', 'runtime': '122 min', 'rating': '8.4', 'votes': '"1,074,179"'}, {'movie_id': 'tt4154796', 'title': 'Avengers: Endgame', 'year': '(2019)', 'rank': '2.', 'genres': 'Action, Adventure, Drama', 'runtime': '181 min', 'rating': '8.4', 'votes': '"945,422"'}]
    """
    print (read_m_by_voting(2018,2020,3))  # This will print a list of top three movies.
```


```python
test_read_m_by_voting()
```

    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=1&title_type=feature&year=2018,2020
    tt7286456
    Joker
    1.
    2019
    122 min
    8.4
    1,271,181
    Crime, Drama, Thriller            
    ===============================
    
    tt4154796
    Avengers: Endgame
    2.
    2019
    181 min
    8.4
    1,121,459
    Action, Adventure, Drama            
    ===============================
    
    tt4154756
    Avengers: Infinity War
    3.
    2018
    149 min
    8.4
    1,073,326
    Action, Adventure, Sci-Fi            
    ===============================
    
    [{'movie_id': 'tt7286456', 'title': 'Joker', 'rank': '1.', 'year': '2019', 'runtime': '122 min', 'rating': '8.4', 'votes': '1,271,181', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt4154796', 'title': 'Avengers: Endgame', 'rank': '2.', 'year': '2019', 'runtime': '181 min', 'rating': '8.4', 'votes': '1,121,459', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt4154756', 'title': 'Avengers: Infinity War', 'rank': '3.', 'year': '2018', 'runtime': '149 min', 'rating': '8.4', 'votes': '1,073,326', 'genres': 'Action, Adventure, Sci-Fi            '}]


# write_movies_csv


```python
import csv
def write_movies_csv(final_list, filename):
    keys = final_list[0].keys()
    with open(filename, 'w', newline='') as output_file:
        dict_writer = csv.DictWriter(output_file, keys)
        dict_writer.writeheader()
        dict_writer.writerows(final_list)

```


```python
# The output of the test_write_movies_csv method is the "IMDb_TopVoted.csv" file.
def test_write_movies_csv(): 
    li = read_m_by_voting(2018, 2020, 500) # To read the top voted 500 movies between 2018 and 2020 from imdb.
    print(li)
    print("================================================================")
    write_movies_csv(li,"IMDb_TopVoted.csv") 

```


```python
test_write_movies_csv()
```

    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=1&title_type=feature&year=2018,2020
    tt7286456
    Joker
    1.
    2019
    122 min
    8.4
    1,271,181
    Crime, Drama, Thriller            
    ===============================
    
    tt4154796
    Avengers: Endgame
    2.
    2019
    181 min
    8.4
    1,121,459
    Action, Adventure, Drama            
    ===============================
    
    tt4154756
    Avengers: Infinity War
    3.
    2018
    149 min
    8.4
    1,073,326
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt6751668
    Parasite
    4.
    2019
    132 min
    8.5
    794,414
    Drama, Thriller            
    ===============================
    
    tt1825683
    Black Panther
    5.
    2018
    134 min
    7.3
    767,098
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt7131622
    Once Upon a Time in Hollywood
    6.
    2019
    161 min
    7.6
    731,841
    Comedy, Drama            
    ===============================
    
    tt8946378
    Knives Out
    7.
    2019
    130 min
    7.9
    625,540
    Comedy, Crime, Drama            
    ===============================
    
    tt8579674
    1917
    8.
    2019
    119 min
    8.2
    585,340
    Action, Drama, War            
    ===============================
    
    tt5463162
    Deadpool 2
    9.
    2018
    119 min
    7.7
    581,115
    Action, Adventure, Comedy            
    ===============================
    
    tt4154664
    Captain Marvel
    10.
    2019
    123 min
    6.8
    559,308
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt1727824
    Bohemian Rhapsody
    11.
    2018
    134 min
    7.9
    537,886
    Biography, Drama, Music            
    ===============================
    
    tt6644200
    A Quiet Place
    12.
    2018
    90 min
    7.5
    528,078
    Drama, Horror, Sci-Fi            
    ===============================
    
    tt4633694
    Spider-Man: Into the Spider-Verse
    13.
    2018
    117 min
    8.4
    524,990
    Animation, Action, Adventure            
    ===============================
    
    tt6723592
    Tenet
    14.
    2020
    150 min
    7.3
    501,142
    Action, Sci-Fi, Thriller            
    ===============================
    
    tt6320628
    Spider-Man: Far from Home
    15.
    2019
    129 min
    7.4
    490,791
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt6966692
    Green Book
    16.
    2018
    130 min
    8.2
    489,304
    Biography, Comedy, Drama            
    ===============================
    
    tt1270797
    Venom
    17.
    2018
    112 min
    6.6
    484,803
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt1477834
    Aquaman
    18.
    2018
    143 min
    6.8
    474,104
    Action, Adventure, Fantasy            
    ===============================
    
    tt2527338
    Star Wars: The Rise Of Skywalker
    19.
    2019
    141 min
    6.5
    450,328
    Action, Adventure, Fantasy            
    ===============================
    
    tt1677720
    Ready Player One
    20.
    2018
    140 min
    7.4
    436,280
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt5095030
    Ant-Man and the Wasp
    21.
    2018
    118 min
    7.0
    401,934
    Action, Adventure, Comedy            
    ===============================
    
    tt1950186
    Ford v Ferrari
    22.
    2019
    152 min
    8.1
    392,046
    Action, Biography, Drama            
    ===============================
    
    tt2584384
    Jojo Rabbit
    23.
    2019
    108 min
    7.9
    388,254
    Comedy, Drama, War            
    ===============================
    
    tt1302006
    The Irishman
    24.
    2019
    209 min
    7.8
    387,450
    Biography, Crime, Drama            
    ===============================
    
    tt1517451
    A Star Is Born
    25.
    2018
    136 min
    7.6
    384,165
    Drama, Music, Romance            
    ===============================
    
    tt3778644
    Solo: A Star Wars Story
    26.
    2018
    135 min
    6.9
    347,374
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt6146586
    John Wick: Chapter 3 - Parabellum
    27.
    2019
    130 min
    7.4
    343,721
    Action, Crime, Thriller            
    ===============================
    
    tt2737304
    Bird Box
    28.
    2018
    124 min
    6.6
    342,273
    Horror, Mystery, Sci-Fi            
    ===============================
    
    tt8367814
    The Gentlemen
    29.
    2019
    113 min
    7.8
    338,310
    Action, Comedy, Crime            
    ===============================
    
    tt0448115
    Shazam!
    30.
    2019
    132 min
    7.0
    338,065
    Action, Adventure, Comedy            
    ===============================
    
    tt4912910
    Mission: Impossible - Fallout
    31.
    2018
    147 min
    7.7
    333,151
    Action, Adventure, Thriller            
    ===============================
    
    tt2948372
    Soul
    32.
    2020
    100 min
    8.0
    328,563
    Animation, Adventure, Comedy            
    ===============================
    
    tt2798920
    Annihilation
    33.
    2018
    115 min
    6.8
    327,214
    Adventure, Drama, Horror            
    ===============================
    
    tt8772262
    Midsommar
    34.
    2019
    148 min
    7.1
    321,719
    Drama, Horror, Mystery            
    ===============================
    
    tt4881806
    Jurassic World: Fallen Kingdom
    35.
    2018
    128 min
    6.1
    317,221
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt7784604
    Hereditary
    36.
    2018
    127 min
    7.3
    316,203
    Drama, Horror, Mystery            
    ===============================
    
    tt7653254
    Marriage Story
    37.
    2019
    137 min
    7.9
    309,090
    Drama, Romance            
    ===============================
    
    tt6857112
    Us
    38.
    2019
    116 min
    6.8
    299,452
    Horror, Mystery, Thriller            
    ===============================
    
    tt3606756
    Incredibles 2
    39.
    2018
    118 min
    7.6
    297,461
    Animation, Action, Adventure            
    ===============================
    
    tt4123430
    Fantastic Beasts: The Crimes of Grindelwald
    40.
    2018
    134 min
    6.5
    283,597
    Adventure, Family, Fantasy            
    ===============================
    
    tt5727208
    Uncut Gems
    41.
    2019
    135 min
    7.4
    282,052
    Crime, Drama, Thriller            
    ===============================
    
    tt0437086
    Alita: Battle Angel
    42.
    2019
    122 min
    7.3
    268,520
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt7126948
    Wonder Woman 1984
    43.
    2020
    151 min
    5.4
    267,271
    Action, Adventure, Fantasy            
    ===============================
    
    tt7349950
    It Chapter Two
    44.
    2019
    169 min
    6.5
    266,323
    Drama, Fantasy, Horror            
    ===============================
    
    tt6139732
    Aladdin
    45.
    2019
    128 min
    6.9
    266,237
    Adventure, Comedy, Family            
    ===============================
    
    tt7349662
    BlacKkKlansman
    46.
    2018
    135 min
    7.5
    264,598
    Biography, Comedy, Crime            
    ===============================
    
    tt1979376
    Toy Story 4
    47.
    2019
    100 min
    7.7
    250,943
    Animation, Adventure, Comedy            
    ===============================
    
    tt7975244
    Jumanji: The Next Level
    48.
    2019
    123 min
    6.7
    250,937
    Action, Adventure, Comedy            
    ===============================
    
    tt9243946
    El Camino: A Breaking Bad Movie
    49.
    2019
    122 min
    7.3
    250,282
    Action, Crime, Drama            
    ===============================
    
    tt6105098
    The Lion King
    50.
    2019
    118 min
    6.8
    245,589
    Animation, Adventure, Drama            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=51&title_type=feature&year=2018,2020
    tt6823368
    Glass
    51.
    2019
    129 min
    6.6
    244,288
    Drama, Horror, Sci-Fi            
    ===============================
    
    tt7713068
    Birds of Prey
    52.
    2020
    109 min
    6.0
    239,392
    Action, Adventure, Comedy            
    ===============================
    
    tt2935510
    Ad Astra
    53.
    2019
    123 min
    6.5
    236,215
    Adventure, Drama, Mystery            
    ===============================
    
    tt2704998
    Game Night
    54.
    2018
    100 min
    6.9
    235,915
    Action, Adventure, Comedy            
    ===============================
    
    tt1051906
    The Invisible Man
    55.
    2020
    124 min
    7.1
    228,905
    Drama, Horror, Mystery            
    ===============================
    
    tt8332922
    A Quiet Place Part II
    56.
    2020
    97 min
    7.2
    227,454
    Drama, Horror, Sci-Fi            
    ===============================
    
    tt8228288
    The Platform
    57.
    2019
    94 min
    7.0
    225,447
    Horror, Sci-Fi, Thriller            
    ===============================
    
    tt5164214
    Ocean's Eight
    58.
    2018
    110 min
    6.3
    220,331
    Action, Comedy, Crime            
    ===============================
    
    tt7984734
    The Lighthouse
    59.
    2019
    109 min
    7.4
    217,503
    Drama, Fantasy, Horror            
    ===============================
    
    tt1365519
    Tomb Raider
    60.
    2018
    119 min
    6.3
    215,365
    Action, Adventure, Fantasy            
    ===============================
    
    tt6806448
    Fast & Furious Presents: Hobbs & Shaw
    61.
    2019
    137 min
    6.5
    213,454
    Action, Adventure, Thriller            
    ===============================
    
    tt3281548
    Little Women
    62.
    2019
    135 min
    7.8
    204,997
    Drama, Romance            
    ===============================
    
    tt8936646
    Extraction
    63.
    2020
    116 min
    6.7
    203,603
    Action, Thriller            
    ===============================
    
    tt5083738
    The Favourite
    64.
    2018
    119 min
    7.5
    201,239
    Biography, Comedy, Drama            
    ===============================
    
    tt5606664
    Doctor Sleep
    65.
    2019
    152 min
    7.3
    194,049
    Drama, Fantasy, Horror            
    ===============================
    
    tt7846844
    Enola Holmes
    66.
    2020
    123 min
    6.6
    189,172
    Action, Adventure, Crime            
    ===============================
    
    tt1213641
    First Man
    67.
    2018
    141 min
    7.3
    189,093
    Biography, Drama, History            
    ===============================
    
    tt6565702
    X-Men: Dark Phoenix
    68.
    2019
    113 min
    5.7
    188,176
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt2873282
    Red Sparrow
    69.
    2018
    140 min
    6.6
    187,340
    Action, Drama, Thriller            
    ===============================
    
    tt6499752
    Upgrade
    70.
    2018
    100 min
    7.5
    186,788
    Action, Sci-Fi, Thriller            
    ===============================
    
    tt3741700
    Godzilla: King of the Monsters
    71.
    2019
    132 min
    6.0
    183,708
    Action, Adventure, Fantasy            
    ===============================
    
    tt1560220
    Zombieland: Double Tap
    72.
    2019
    99 min
    6.7
    180,853
    Action, Comedy, Horror            
    ===============================
    
    tt6450804
    Terminator: Dark Fate
    73.
    2019
    128 min
    6.2
    178,582
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt1070874
    The Trial of the Chicago 7
    74.
    2020
    129 min
    7.7
    177,721
    Drama, History, Thriller            
    ===============================
    
    tt8106534
    6 Underground
    75.
    2019
    128 min
    6.1
    177,605
    Action, Thriller            
    ===============================
    
    tt2066051
    Rocketman
    76.
    2019
    121 min
    7.3
    175,928
    Biography, Drama, Musical            
    ===============================
    
    tt3104988
    Crazy Rich Asians
    77.
    2018
    120 min
    6.9
    173,422
    Comedy, Drama, Romance            
    ===============================
    
    tt9620292
    Promising Young Woman
    78.
    2020
    113 min
    7.5
    173,222
    Crime, Drama, Mystery            
    ===============================
    
    tt4520988
    Frozen II
    79.
    2019
    103 min
    6.8
    173,057
    Animation, Adventure, Comedy            
    ===============================
    
    tt4779682
    The Meg
    80.
    2018
    113 min
    5.6
    172,473
    Action, Horror, Sci-Fi            
    ===============================
    
    tt5104604
    Isle of Dogs
    81.
    2018
    101 min
    7.8
    169,740
    Animation, Adventure, Comedy            
    ===============================
    
    tt2231461
    Rampage
    82.
    2018
    107 min
    6.1
    169,347
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt4701182
    Bumblebee
    83.
    2018
    114 min
    6.7
    166,274
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt7556122
    The Old Guard
    84.
    2020
    125 min
    6.6
    165,726
    Action, Thriller            
    ===============================
    
    tt5884052
    Pokémon Detective Pikachu
    85.
    2019
    104 min
    6.5
    164,134
    Action, Adventure, Comedy            
    ===============================
    
    tt7668870
    Searching
    86.
    2018
    102 min
    7.6
    163,657
    Drama, Horror, Mystery            
    ===============================
    
    tt5848272
    Ralph Breaks the Internet
    87.
    2018
    112 min
    7.0
    162,112
    Animation, Adventure, Comedy            
    ===============================
    
    tt1502397
    Bad Boys for Life
    88.
    2020
    124 min
    6.5
    161,393
    Action, Comedy, Crime            
    ===============================
    
    tt9770150
    Nomadland
    89.
    2020
    107 min
    7.3
    161,208
    Drama            
    ===============================
    
    tt3766354
    The Equalizer 2
    90.
    2018
    121 min
    6.7
    161,070
    Action, Crime, Thriller            
    ===============================
    
    tt9484998
    Palm Springs
    91.
    2020
    90 min
    7.4
    159,737
    Comedy, Fantasy, Mystery            
    ===============================
    
    tt6155172
    Roma
    92.
    2018
    135 min
    7.7
    158,974
    Drama            
    ===============================
    
    tt10288566
    Another Round
    93.
    2020
    117 min
    7.7
    157,969
    Comedy, Drama            
    ===============================
    
    tt1502407
    Halloween
    94.
    2018
    106 min
    6.5
    156,320
    Crime, Horror, Thriller            
    ===============================
    
    tt8079248
    Yesterday
    95.
    2019
    116 min
    6.8
    153,653
    Comedy, Fantasy, Music            
    ===============================
    
    tt10272386
    The Father
    96.
    2020
    97 min
    8.2
    152,850
    Drama, Mystery            
    ===============================
    
    tt7798634
    Ready or Not
    97.
    2019
    95 min
    6.8
    152,395
    Action, Comedy, Horror            
    ===============================
    
    tt6628394
    Bad Times at the El Royale
    98.
    2018
    141 min
    7.1
    151,350
    Crime, Drama, Mystery            
    ===============================
    
    tt7040874
    A Simple Favor
    99.
    2018
    117 min
    6.8
    150,432
    Comedy, Crime, Mystery            
    ===============================
    
    tt4729430
    Klaus
    100.
    2019
    96 min
    8.1
    150,008
    Animation, Adventure, Comedy            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=101&title_type=feature&year=2018,2020
    tt7146812
    Onward
    101.
    2020
    102 min
    7.4
    149,883
    Animation, Adventure, Comedy            
    ===============================
    
    tt5052474
    Sicario: Day of the Soldado
    102.
    2018
    122 min
    7.0
    149,090
    Action, Crime, Drama            
    ===============================
    
    tt6412452
    The Ballad of Buster Scruggs
    103.
    2018
    133 min
    7.3
    148,746
    Comedy, Drama, Musical            
    ===============================
    
    tt4566758
    Mulan
    104.
    2020
    115 min
    5.7
    148,665
    Action, Adventure, Drama            
    ===============================
    
    tt6266538
    Vice
    105.
    2018
    132 min
    7.2
    148,538
    Biography, Comedy, Drama            
    ===============================
    
    tt5814060
    The Nun
    106.
    2018
    96 min
    5.3
    142,259
    Horror, Mystery, Thriller            
    ===============================
    
    tt4500922
    Maze Runner: The Death Cure
    107.
    2018
    143 min
    6.2
    141,412
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt13143964
    Borat Subsequent Moviefilm
    108.
    2020
    95 min
    6.6
    141,407
    Comedy            
    ===============================
    
    tt3794354
    Sonic the Hedgehog
    109.
    2020
    99 min
    6.5
    140,177
    Action, Adventure, Comedy            
    ===============================
    
    tt7959026
    The Mule
    110.
    2018
    116 min
    7.0
    138,359
    Crime, Drama, Thriller            
    ===============================
    
    tt7395114
    The Devil All the Time
    111.
    2020
    138 min
    7.1
    135,582
    Crime, Drama, Thriller            
    ===============================
    
    tt2283336
    Men in Black: International
    112.
    2019
    114 min
    5.6
    135,465
    Action, Adventure, Comedy            
    ===============================
    
    tt2854926
    Tag
    113.
    2018
    100 min
    6.5
    134,919
    Action, Comedy            
    ===============================
    
    tt3829266
    The Predator
    114.
    2018
    107 min
    5.3
    134,630
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt9893250
    I Care a Lot
    115.
    2020
    118 min
    6.3
    133,361
    Comedy, Crime, Drama            
    ===============================
    
    tt8110330
    Dil Bechara
    116.
    2020
    101 min
    8.1
    131,177
    Comedy, Drama, Romance            
    ===============================
    
    tt7984766
    The King
    117.
    2019
    140 min
    7.3
    130,503
    Biography, Drama, History            
    ===============================
    
    tt2386490
    How to Train Your Dragon: The Hidden World
    118.
    2019
    104 min
    7.4
    129,945
    Animation, Action, Adventure            
    ===============================
    
    tt1488606
    Triple Frontier
    119.
    2019
    125 min
    6.4
    129,554
    Action, Thriller            
    ===============================
    
    tt1618434
    Murder Mystery
    120.
    2019
    97 min
    6.0
    129,227
    Action, Comedy, Crime            
    ===============================
    
    tt5363618
    Sound of Metal
    121.
    2019
    120 min
    7.7
    128,340
    Drama, Music            
    ===============================
    
    tt2222042
    Love and Monsters
    122.
    2020
    109 min
    6.9
    128,171
    Action, Adventure, Comedy            
    ===============================
    
    tt1571234
    Mortal Engines
    123.
    2018
    128 min
    6.1
    127,438
    Action, Adventure, Fantasy            
    ===============================
    
    tt9495224
    Black Mirror: Bandersnatch
    124.
    2018
    90 min
    7.1
    127,379
    Drama, Fantasy, Mystery            
    ===============================
    
    tt6343314
    Creed II
    125.
    2018
    130 min
    7.1
    126,612
    Drama, Sport            
    ===============================
    
    tt8404614
    The Two Popes
    126.
    2019
    125 min
    7.6
    124,359
    Biography, Comedy, Drama            
    ===============================
    
    tt5886046
    Escape Room
    127.
    2019
    99 min
    6.4
    123,696
    Action, Adventure, Horror            
    ===============================
    
    tt5758778
    Skyscraper
    128.
    2018
    102 min
    5.8
    123,391
    Action, Adventure, Thriller            
    ===============================
    
    tt8847712
    The French Dispatch
    129.
    2020
    107 min
    7.1
    121,353
    Comedy, Drama, Romance            
    ===============================
    
    tt1489887
    Booksmart
    130.
    2019
    102 min
    7.1
    120,057
    Comedy            
    ===============================
    
    tt2557478
    Pacific Rim: Uprising
    131.
    2018
    111 min
    5.6
    118,362
    Action, Adventure, Fantasy            
    ===============================
    
    tt7737786
    Greenland
    132.
    2020
    119 min
    6.4
    117,784
    Action, Thriller            
    ===============================
    
    tt1590193
    The Commuter
    133.
    2018
    104 min
    6.3
    116,801
    Action, Mystery, Thriller            
    ===============================
    
    tt10189514
    Soorarai Pottru
    134.
    2020
    153 min
    8.7
    116,747
    Drama            
    ===============================
    
    tt5164432
    Love, Simon
    135.
    2018
    110 min
    7.5
    115,880
    Comedy, Drama, Romance            
    ===============================
    
    tt6394270
    Bombshell
    136.
    2019
    109 min
    6.8
    115,850
    Biography, Drama            
    ===============================
    
    tt8244784
    The Hunt
    137.
    2020
    90 min
    6.5
    115,334
    Action, Horror, Thriller            
    ===============================
    
    tt1025100
    Gemini Man
    138.
    2019
    117 min
    5.7
    114,286
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt1259528
    Den of Thieves
    139.
    2018
    140 min
    7.0
    113,067
    Action, Crime, Drama            
    ===============================
    
    tt2139881
    Long Shot
    140.
    2019
    125 min
    6.8
    112,167
    Comedy, Romance            
    ===============================
    
    tt3846674
    To All the Boys I've Loved Before
    141.
    2018
    99 min
    7.0
    108,777
    Comedy, Drama, Romance            
    ===============================
    
    tt7401588
    Instant Family
    142.
    2018
    118 min
    7.3
    108,012
    Comedy, Drama            
    ===============================
    
    tt4777008
    Maleficent: Mistress of Evil
    143.
    2019
    119 min
    6.6
    106,127
    Adventure, Family, Fantasy            
    ===============================
    
    tt2548396
    The Cloverfield Paradox
    144.
    2018
    102 min
    5.5
    106,127
    Action, Horror, Sci-Fi            
    ===============================
    
    tt5503686
    Hustlers
    145.
    2019
    110 min
    6.3
    100,913
    Comedy, Crime, Drama            
    ===============================
    
    tt4530422
    Overlord
    146.
    2018
    110 min
    6.6
    100,051
    Action, Horror, Sci-Fi            
    ===============================
    
    tt1206885
    Rambo: Last Blood
    147.
    2019
    89 min
    6.1
    99,338
    Action, Crime, Thriller            
    ===============================
    
    tt7752126
    Brightburn
    148.
    2019
    90 min
    6.1
    99,009
    Drama, Horror, Mystery            
    ===============================
    
    tt6189022
    Angel Has Fallen
    149.
    2019
    121 min
    6.4
    98,617
    Action, Thriller            
    ===============================
    
    tt4218572
    Widows
    150.
    2018
    129 min
    6.8
    98,427
    Crime, Drama, Thriller            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=151&title_type=feature&year=2018,2020
    tt6048922
    Greyhound
    151.
    2020
    91 min
    7.0
    97,966
    Action, Drama, History            
    ===============================
    
    tt6911608
    Mamma Mia! Here We Go Again
    152.
    2018
    114 min
    6.6
    96,271
    Comedy, Musical, Romance            
    ===============================
    
    tt7886848
    Sadak 2
    153.
    2020
    133 min
    1.1
    95,972
    Action, Drama            
    ===============================
    
    tt2481498
    Extremely Wicked, Shockingly Evil and Vile
    154.
    2019
    110 min
    6.7
    95,250
    Biography, Crime, Drama            
    ===============================
    
    tt8580274
    Eurovision Song Contest: The Story of Fire Saga
    155.
    2020
    123 min
    6.5
    94,634
    Comedy, Musical            
    ===============================
    
    tt8503618
    Hamilton
    156.
    2020
    160 min
    8.4
    94,561
    Biography, Drama, History            
    ===============================
    
    tt8108198
    Andhadhun
    157.
    2018
    139 min
    8.2
    93,926
    Comedy, Crime, Music            
    ===============================
    
    tt0837563
    Pet Sematary
    158.
    2019
    101 min
    5.7
    92,967
    Horror, Mystery, Thriller            
    ===============================
    
    tt1860242
    The Highwaymen
    159.
    2019
    132 min
    6.9
    92,254
    Biography, Crime, Drama            
    ===============================
    
    tt8613070
    Portrait of a Lady on Fire
    160.
    2019
    122 min
    8.1
    92,088
    Drama, Romance            
    ===============================
    
    tt6292852
    I Am Mother
    161.
    2019
    113 min
    6.7
    91,855
    Drama, Mystery, Sci-Fi            
    ===============================
    
    tt2274648
    Hellboy
    162.
    2019
    120 min
    5.2
    91,189
    Action, Adventure, Fantasy            
    ===============================
    
    tt4364194
    The Peanut Butter Falcon
    163.
    2019
    97 min
    7.6
    90,968
    Adventure, Comedy, Drama            
    ===============================
    
    tt4139588
    Polar
    164.
    2019
    118 min
    6.3
    90,874
    Action, Thriller            
    ===============================
    
    tt10886166
    365 Days
    165.
    2020
    114 min
    3.3
    90,341
    Drama, Romance            
    ===============================
    
    tt8267604
    Capernaum
    166.
    2018
    126 min
    8.4
    89,953
    Drama            
    ===============================
    
    tt9071322
    Dark Waters
    167.
    2019
    126 min
    7.6
    89,374
    Biography, Drama, History            
    ===============================
    
    tt7550000
    Project Power
    168.
    2020
    113 min
    6.0
    89,277
    Action, Crime, Sci-Fi            
    ===============================
    
    tt8629748
    Spenser Confidential
    169.
    2020
    111 min
    6.2
    89,116
    Action, Comedy, Drama            
    ===============================
    
    tt5028340
    Mary Poppins Returns
    170.
    2018
    130 min
    6.7
    88,841
    Adventure, Comedy, Family            
    ===============================
    
    tt3513548
    Richard Jewell
    171.
    2019
    131 min
    7.5
    88,079
    Biography, Crime, Drama            
    ===============================
    
    tt6878306
    News of the World
    172.
    2020
    118 min
    6.8
    87,541
    Action, Adventure, Drama            
    ===============================
    
    tt7939766
    I'm Thinking of Ending Things
    173.
    2020
    134 min
    6.6
    87,260
    Drama, Thriller            
    ===============================
    
    tt7838252
    K.G.F: Chapter 1
    174.
    2018
    156 min
    8.2
    86,915
    Action, Crime, Drama            
    ===============================
    
    tt6924650
    Midway
    175.
    2019
    138 min
    6.7
    86,345
    Action, Drama, History            
    ===============================
    
    tt8364368
    Crawl
    176.
    2019
    87 min
    6.1
    85,343
    Action, Adventure, Horror            
    ===============================
    
    tt3799232
    The Kissing Booth
    177.
    2018
    105 min
    5.9
    84,974
    Comedy, Romance            
    ===============================
    
    tt10539608
    The Midnight Sky
    178.
    2020
    118 min
    5.6
    83,774
    Adventure, Drama, Sci-Fi            
    ===============================
    
    tt5774060
    Underwater
    179.
    2020
    95 min
    5.8
    83,624
    Action, Horror, Sci-Fi            
    ===============================
    
    tt7456310
    Anna
    180.
    2019
    118 min
    6.6
    82,956
    Action, Thriller            
    ===============================
    
    tt1413492
    12 Strong
    181.
    2018
    130 min
    6.5
    82,826
    Action, Drama, History            
    ===============================
    
    tt6513120
    Fighting with My Family
    182.
    2019
    108 min
    7.1
    81,991
    Biography, Comedy, Drama            
    ===============================
    
    tt10633456
    Minari
    183.
    2020
    115 min
    7.4
    81,799
    Drama            
    ===============================
    
    tt10554232
    Dara of Jasenovac
    184.
    2020
    130 min
    8.2
    81,058
    Drama, War            
    ===============================
    
    tt3224458
    A Beautiful Day in the Neighborhood
    185.
    2019
    109 min
    7.2
    80,712
    Biography, Drama            
    ===============================
    
    tt4560436
    Mile 22
    186.
    2018
    94 min
    6.1
    80,548
    Action, Thriller            
    ===============================
    
    tt5688932
    Sorry to Bother You
    187.
    2018
    112 min
    6.9
    80,419
    Comedy, Drama, Fantasy            
    ===============================
    
    tt4682266
    The New Mutants
    188.
    2020
    94 min
    5.3
    80,325
    Action, Horror, Mystery            
    ===============================
    
    tt2531344
    Blockers
    189.
    2018
    102 min
    6.2
    80,292
    Comedy, Drama            
    ===============================
    
    tt6998518
    Mandy
    190.
    2018
    121 min
    6.5
    80,264
    Action, Fantasy, Horror            
    ===============================
    
    tt8633478
    Run
    191.
    2020
    90 min
    6.7
    80,236
    Mystery, Thriller            
    ===============================
    
    tt1034415
    Suspiria
    192.
    2018
    152 min
    6.7
    79,504
    Drama, Fantasy, Horror            
    ===============================
    
    tt8695030
    The Dead Don't Die
    193.
    2019
    104 min
    5.4
    79,272
    Comedy, Fantasy, Horror            
    ===============================
    
    tt8623904
    Last Christmas
    194.
    2019
    103 min
    6.5
    78,718
    Comedy, Drama, Fantasy            
    ===============================
    
    tt4575576
    Christopher Robin
    195.
    2018
    104 min
    7.2
    78,659
    Adventure, Comedy, Drama            
    ===============================
    
    tt1634106
    Bloodshot
    196.
    2020
    109 min
    5.7
    78,504
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt6663582
    The Spy Who Dumped Me
    197.
    2018
    117 min
    6.0
    78,369
    Action, Adventure, Comedy            
    ===============================
    
    tt1226837
    Beautiful Boy
    198.
    2018
    120 min
    7.3
    78,369
    Biography, Drama            
    ===============================
    
    tt3387520
    Scary Stories to Tell in the Dark
    199.
    2019
    108 min
    6.2
    77,754
    Adventure, Horror, Mystery            
    ===============================
    
    tt4003440
    The House That Jack Built
    200.
    2018
    152 min
    6.8
    77,743
    Crime, Drama, Horror            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=201&title_type=feature&year=2018,2020
    tt8075192
    Shoplifters
    201.
    2018
    121 min
    7.9
    77,220
    Crime, Drama, Thriller            
    ===============================
    
    tt8350360
    Annabelle Comes Home
    202.
    2019
    106 min
    5.9
    76,679
    Horror, Mystery, Thriller            
    ===============================
    
    tt3861390
    Dumbo
    203.
    2019
    112 min
    6.3
    76,633
    Adventure, Family, Fantasy            
    ===============================
    
    tt7014006
    Eighth Grade
    204.
    2018
    93 min
    7.4
    76,456
    Comedy, Drama            
    ===============================
    
    tt4332232
    Fractured
    205.
    2019
    99 min
    6.4
    76,347
    Thriller            
    ===============================
    
    tt10618286
    Mank
    206.
    2020
    131 min
    6.8
    75,796
    Biography, Comedy, Drama            
    ===============================
    
    tt8155288
    Happy Death Day 2U
    207.
    2019
    100 min
    6.2
    75,531
    Comedy, Horror, Mystery            
    ===============================
    
    tt5220122
    Hotel Transylvania 3: Summer Vacation
    208.
    2018
    97 min
    6.3
    75,163
    Animation, Adventure, Comedy            
    ===============================
    
    tt6921996
    Johnny English Strikes Again
    209.
    2018
    89 min
    6.2
    75,119
    Action, Adventure, Comedy            
    ===============================
    
    tt6977338
    Good Boys
    210.
    2019
    90 min
    6.7
    74,922
    Adventure, Comedy            
    ===============================
    
    tt4532826
    Robin Hood
    211.
    2018
    116 min
    5.3
    74,567
    Action, Adventure, Drama            
    ===============================
    
    tt5033998
    Charlie's Angels
    212.
    2019
    118 min
    4.9
    74,246
    Action, Adventure, Comedy            
    ===============================
    
    tt10280276
    Coolie No. 1
    213.
    2020
    134 min
    4.2
    73,650
    Action, Comedy, Romance            
    ===============================
    
    tt2990140
    The Christmas Chronicles
    214.
    2018
    104 min
    7.0
    73,326
    Adventure, Comedy, Family            
    ===============================
    
    tt6679794
    Outlaw King
    215.
    2018
    121 min
    6.9
    73,019
    Action, Biography, Drama            
    ===============================
    
    tt1137450
    Death Wish
    216.
    2018
    107 min
    6.3
    72,793
    Action, Crime, Drama            
    ===============================
    
    tt2709692
    The Grinch
    217.
    2018
    85 min
    6.3
    71,456
    Animation, Comedy, Family            
    ===============================
    
    tt2452244
    Isn't It Romantic
    218.
    2019
    89 min
    5.9
    70,531
    Comedy, Fantasy, Musical            
    ===============================
    
    tt5719748
    Cold Pursuit
    219.
    2019
    119 min
    6.2
    69,958
    Action, Comedy, Crime            
    ===============================
    
    tt8359848
    Climax
    220.
    2018
    97 min
    6.9
    69,470
    Drama, Horror, Music            
    ===============================
    
    tt10059518
    Unhinged
    221.
    2020
    90 min
    6.0
    69,304
    Action, Thriller            
    ===============================
    
    tt9686708
    The King of Staten Island
    222.
    2020
    136 min
    7.1
    68,697
    Comedy, Drama            
    ===============================
    
    tt4916630
    Just Mercy
    223.
    2019
    137 min
    7.6
    68,287
    Biography, Crime, Drama            
    ===============================
    
    tt7638348
    Boss Level
    224.
    2020
    100 min
    6.8
    68,283
    Action, Adventure, Comedy            
    ===============================
    
    tt8688634
    21 Bridges
    225.
    2019
    99 min
    6.6
    68,062
    Action, Crime, Drama            
    ===============================
    
    tt6472976
    Five Feet Apart
    226.
    2019
    116 min
    7.2
    67,916
    Drama, Romance            
    ===============================
    
    tt3513498
    The Lego Movie 2: The Second Part
    227.
    2019
    107 min
    6.6
    67,523
    Animation, Action, Adventure            
    ===============================
    
    tt4477536
    Fifty Shades Freed
    228.
    2018
    105 min
    4.5
    66,784
    Drama, Romance, Thriller            
    ===============================
    
    tt9866072
    Holidate
    229.
    2020
    104 min
    6.1
    66,699
    Comedy, Romance            
    ===============================
    
    tt5613484
    Mid90s
    230.
    2018
    85 min
    7.3
    66,371
    Comedy, Drama            
    ===============================
    
    tt7282468
    Burning
    231.
    2018
    148 min
    7.5
    65,679
    Drama, Mystery, Thriller            
    ===============================
    
    tt6850820
    Peppermint
    232.
    2018
    101 min
    6.5
    65,629
    Action, Thriller            
    ===============================
    
    tt4971344
    The Sisters Brothers
    233.
    2018
    122 min
    6.9
    65,472
    Drama, Western            
    ===============================
    
    tt6673612
    Dolittle
    234.
    2020
    101 min
    5.6
    65,172
    Adventure, Comedy, Family            
    ===============================
    
    tt8637428
    The Farewell
    235.
    2019
    100 min
    7.5
    64,930
    Comedy, Drama            
    ===============================
    
    tt5461944
    Hotel Mumbai
    236.
    2018
    123 min
    7.6
    64,813
    Action, Drama, History            
    ===============================
    
    tt6133466
    The First Purge
    237.
    2018
    97 min
    5.2
    64,779
    Action, Horror, Sci-Fi            
    ===============================
    
    tt8291224
    Uri: The Surgical Strike
    238.
    2019
    138 min
    8.2
    64,482
    Action, Drama, History            
    ===============================
    
    tt1846589
    Hunter Killer
    239.
    2018
    121 min
    6.6
    64,208
    Action, Thriller            
    ===============================
    
    tt4244998
    Alpha
    240.
    2018
    96 min
    6.6
    63,094
    Action, Adventure, Drama            
    ===============================
    
    tt5113040
    The Secret Life of Pets 2
    241.
    2019
    86 min
    6.4
    62,876
    Animation, Adventure, Comedy            
    ===============================
    
    tt2388771
    Mowgli: Legend of the Jungle
    242.
    2018
    104 min
    6.5
    62,531
    Adventure, Drama, Fantasy            
    ===============================
    
    tt6902676
    Guns Akimbo
    243.
    2019
    98 min
    6.3
    62,427
    Action, Comedy, Crime            
    ===============================
    
    tt7043012
    Velvet Buzzsaw
    244.
    2019
    113 min
    5.7
    61,942
    Horror, Mystery, Thriller            
    ===============================
    
    tt8368512
    The Courier
    245.
    2020
    112 min
    7.2
    61,842
    Drama, History, Thriller            
    ===============================
    
    tt5726086
    Insidious: The Last Key
    246.
    2018
    103 min
    5.7
    61,502
    Horror, Mystery, Thriller            
    ===============================
    
    tt8526872
    Dolemite Is My Name
    247.
    2019
    118 min
    7.2
    61,326
    Biography, Comedy, Drama            
    ===============================
    
    tt1298644
    The Hustle
    248.
    2019
    93 min
    5.4
    60,425
    Comedy, Crime            
    ===============================
    
    tt3892172
    Leave No Trace
    249.
    2018
    109 min
    7.1
    60,366
    Adventure, Drama            
    ===============================
    
    tt8368406
    Vivarium
    250.
    2019
    97 min
    5.8
    60,196
    Horror, Mystery, Sci-Fi            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=251&title_type=feature&year=2018,2020
    tt5610554
    Tully
    251.
    2018
    95 min
    6.9
    59,824
    Comedy, Drama, Mystery            
    ===============================
    
    tt6475714
    Monster Hunter
    252.
    2020
    103 min
    5.2
    59,654
    Action, Adventure, Fantasy            
    ===============================
    
    tt6742252
    The Guilty
    253.
    2018
    85 min
    7.5
    59,534
    Crime, Drama, Thriller            
    ===============================
    
    tt10919380
    Freaky
    254.
    2020
    102 min
    6.3
    59,013
    Comedy, Horror, Thriller            
    ===============================
    
    tt8291806
    Pain and Glory
    255.
    2019
    113 min
    7.5
    58,732
    Drama            
    ===============================
    
    tt11032374
    Demon Slayer the Movie: Mugen Train
    256.
    2020
    117 min
    8.2
    58,154
    Animation, Action, Adventure            
    ===============================
    
    tt0385887
    Motherless Brooklyn
    257.
    2019
    144 min
    6.8
    58,022
    Crime, Drama, Mystery            
    ===============================
    
    tt6543652
    Cold War
    258.
    2018
    89 min
    7.5
    57,613
    Drama, Music, Romance            
    ===============================
    
    tt10350922
    Laxmii
    259.
    2020
    141 min
    2.6
    57,508
    Action, Comedy, Horror            
    ===============================
    
    tt6306064
    Adrift
    260.
    2018
    96 min
    6.6
    57,445
    Action, Adventure, Biography            
    ===============================
    
    tt4687108
    In the Tall Grass
    261.
    2019
    101 min
    5.4
    57,272
    Horror, Mystery, Sci-Fi            
    ===============================
    
    tt6772950
    Truth or Dare
    262.
    2018
    100 min
    5.2
    57,124
    Horror, Thriller            
    ===============================
    
    tt7374948
    Always Be My Maybe
    263.
    2019
    101 min
    6.8
    57,079
    Comedy, Romance            
    ===============================
    
    tt6398184
    Downton Abbey
    264.
    2019
    122 min
    7.4
    56,946
    Drama, Romance            
    ===============================
    
    tt6908274
    Mirage
    265.
    2018
    128 min
    7.4
    56,709
    Drama, Fantasy, Mystery            
    ===============================
    
    tt4126476
    After
    266.
    2019
    105 min
    5.3
    56,295
    Drama, Romance            
    ===============================
    
    tt8784956
    Ava
    267.
    V2020
    96 min
    5.4
    56,228
    Action, Drama, Thriller            
    ===============================
    
    tt6452574
    Sanju
    268.
    2018
    155 min
    7.6
    55,342
    Biography, Comedy, Drama            
    ===============================
    
    tt7668842
    Enes Batur
    269.
    2018
    110 min
    2.0
    55,290
    Comedy            
    ===============================
    
    tt7825208
    Marighella
    270.
    2019
    155 min
    6.6
    55,248
    Action, Drama, History            
    ===============================
    
    tt9052870
    Chhichhore
    271.
    2019
    143 min
    8.3
    54,935
    Comedy, Drama, Romance            
    ===============================
    
    tt5304992
    Set It Up
    272.
    2018
    105 min
    6.5
    54,919
    Comedy, Romance            
    ===============================
    
    tt9214832
    Emma.
    273.
    2020
    124 min
    6.7
    54,890
    Comedy, Drama, Romance            
    ===============================
    
    tt5968394
    Captive State
    274.
    2019
    109 min
    6.0
    54,834
    Action, Horror, Sci-Fi            
    ===============================
    
    tt6791096
    I Feel Pretty
    275.
    2018
    110 min
    5.6
    54,418
    Comedy, Romance            
    ===============================
    
    tt5834262
    Hotel Artemis
    276.
    2018
    94 min
    6.1
    54,226
    Action, Crime, Drama            
    ===============================
    
    tt4463894
    Shaft
    277.
    2019
    111 min
    6.4
    54,188
    Action, Comedy, Crime            
    ===============================
    
    tt7958736
    Ma
    278.
    2019
    99 min
    5.6
    54,006
    Horror, Mystery, Thriller            
    ===============================
    
    tt9731534
    The Night House
    279.
    2020
    107 min
    6.5
    53,961
    Horror, Mystery, Thriller            
    ===============================
    
    tt8110640
    In the Shadow of the Moon
    280.
    2019
    115 min
    6.2
    53,927
    Action, Crime, Mystery            
    ===============================
    
    tt10514222
    Ma Rainey's Black Bottom
    281.
    2020
    94 min
    6.9
    53,918
    Drama, Music            
    ===============================
    
    tt5246700
    How It Ends
    282.
    2018
    113 min
    5.0
    53,821
    Action, Drama, Horror            
    ===============================
    
    tt6217306
    Apostle
    283.
    2018
    130 min
    6.3
    53,813
    Drama, Fantasy, Horror            
    ===============================
    
    tt3201640
    Extinction
    284.
    2018
    95 min
    5.8
    53,564
    Action, Drama, Sci-Fi            
    ===============================
    
    tt0983946
    Fantasy Island
    285.
    2020
    109 min
    4.9
    53,057
    Fantasy, Horror, Mystery            
    ===============================
    
    tt4595882
    Can You Ever Forgive Me?
    286.
    2018
    106 min
    7.1
    52,828
    Biography, Comedy, Crime            
    ===============================
    
    tt6820256
    Arctic
    287.
    2018
    98 min
    6.8
    52,823
    Adventure, Drama            
    ===============================
    
    tt8663516
    Child's Play
    288.
    2019
    90 min
    5.7
    52,653
    Drama, Horror, Sci-Fi            
    ===============================
    
    tt2119543
    The House with a Clock in Its Walls
    289.
    2018
    105 min
    6.0
    52,331
    Comedy, Family, Fantasy            
    ===============================
    
    tt9777644
    Da 5 Bloods
    290.
    2020
    154 min
    6.5
    51,906
    Adventure, Drama, War            
    ===============================
    
    tt5697572
    Cats
    291.
    2019
    110 min
    2.8
    51,739
    Comedy, Drama, Family            
    ===============================
    
    tt10682266
    Hubie Halloween
    292.
    2020
    103 min
    5.2
    51,649
    Comedy, Mystery            
    ===============================
    
    tt5865326
    The Laundromat
    293.
    2019
    96 min
    6.3
    51,433
    Comedy, Crime, Drama            
    ===============================
    
    tt2328900
    Mary Queen of Scots
    294.
    2018
    124 min
    6.3
    51,188
    Biography, Drama, History            
    ===============================
    
    tt11655202
    Riders of Justice
    295.
    2020
    116 min
    7.5
    51,132
    Action, Comedy, Drama            
    ===============================
    
    tt4913966
    The Curse of La Llorona
    296.
    2019
    93 min
    5.2
    51,113
    Horror, Mystery, Thriller            
    ===============================
    
    tt7549996
    Judy
    297.
    2019
    118 min
    6.8
    51,097
    Biography, Drama, Romance            
    ===============================
    
    tt1838556
    Honest Thief
    298.
    2020
    99 min
    6.0
    50,985
    Action, Thriller            
    ===============================
    
    tt11161474
    Pieces of a Woman
    299.
    2020
    126 min
    7.0
    50,395
    Drama            
    ===============================
    
    tt6079516
    I See You
    300.
    2019
    98 min
    6.8
    50,330
    Crime, Drama, Horror            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=301&title_type=feature&year=2018,2020
    tt0800325
    The Dirt
    301.
    2019
    107 min
    7.0
    50,318
    Biography, Comedy, Drama            
    ===============================
    
    tt5783956
    When We First Met
    302.
    2018
    97 min
    6.4
    49,980
    Comedy, Fantasy, Romance            
    ===============================
    
    tt5814534
    Spies in Disguise
    303.
    2019
    102 min
    6.8
    49,893
    Animation, Action, Adventure            
    ===============================
    
    tt7504726
    The Call of the Wild
    304.
    2020
    100 min
    6.7
    49,773
    Adventure, Drama, Family            
    ===============================
    
    tt5116302
    Togo
    305.
    2019
    113 min
    7.9
    49,470
    Adventure, Biography, Drama            
    ===============================
    
    tt8239946
    Tumbbad
    306.
    2018
    104 min
    8.2
    49,389
    Drama, Fantasy, Horror            
    ===============================
    
    tt7125860
    If Beale Street Could Talk
    307.
    2018
    119 min
    7.1
    49,341
    Drama, Romance            
    ===============================
    
    tt10431500
    Miracle in Cell No. 7
    308.
    2019
    132 min
    8.2
    49,299
    Drama            
    ===============================
    
    tt5073642
    Color Out of Space
    309.
    2019
    111 min
    6.2
    48,856
    Horror, Mystery, Sci-Fi            
    ===============================
    
    tt7347846
    The Lodge
    310.
    2019
    108 min
    6.0
    48,586
    Drama, Horror, Mystery            
    ===============================
    
    tt5431890
    Official Secrets
    311.
    2019
    112 min
    7.3
    48,425
    Biography, Crime, Drama            
    ===============================
    
    tt5177088
    The Girl in the Spider's Web
    312.
    2018
    115 min
    6.1
    48,259
    Action, Crime, Drama            
    ===============================
    
    tt1086064
    Bill & Ted Face the Music
    313.
    2020
    91 min
    5.9
    48,250
    Adventure, Comedy, Music            
    ===============================
    
    tt7734218
    Stuber
    314.
    2019
    93 min
    6.2
    48,032
    Action, Comedy, Crime            
    ===============================
    
    tt1289403
    The Guernsey Literary and Potato Peel Pie Society
    315.
    2018
    124 min
    7.3
    47,346
    Drama, Romance, War            
    ===============================
    
    tt6491178
    Dragged Across Concrete
    316.
    2018
    159 min
    6.9
    47,094
    Action, Thriller            
    ===============================
    
    tt8544498
    The Way Back
    317.
    2020
    108 min
    6.7
    47,008
    Drama, Sport            
    ===============================
    
    tt7153766
    Unsane
    318.
    2018
    98 min
    6.4
    46,973
    Drama, Horror, Mystery            
    ===============================
    
    tt7431594
    Race 3
    319.
    2018
    160 min
    1.9
    46,888
    Action, Crime, Thriller            
    ===============================
    
    tt2837574
    The Old Man & the Gun
    320.
    2018
    93 min
    6.7
    46,645
    Biography, Comedy, Crime            
    ===============================
    
    tt5932728
    The Professor and the Madman
    321.
    2019
    124 min
    7.2
    46,563
    Biography, Drama, History            
    ===============================
    
    tt7315484
    The Silence
    322.
    2019
    90 min
    5.3
    45,999
    Drama, Horror, Sci-Fi            
    ===============================
    
    tt6212478
    American Animals
    323.
    2018
    116 min
    7.0
    45,508
    Biography, Crime, Drama            
    ===============================
    
    tt1620680
    A Wrinkle in Time
    324.
    2018
    109 min
    4.2
    45,394
    Adventure, Family, Fantasy            
    ===============================
    
    tt5397194
    Anon
    325.
    2018
    100 min
    6.1
    45,109
    Crime, Mystery, Sci-Fi            
    ===============================
    
    tt7060344
    Raatchasan
    326.
    2018
    170 min
    8.3
    44,968
    Crime, Drama, Mystery            
    ===============================
    
    tt7139936
    A Rainy Day in New York
    327.
    2019
    92 min
    6.5
    44,920
    Comedy, Romance            
    ===============================
    
    tt7772580
    The Perfection
    328.
    2018
    90 min
    6.2
    44,255
    Drama, Horror, Music            
    ===============================
    
    tt8236336
    The Report
    329.
    2019
    119 min
    7.2
    44,236
    Biography, Crime, Drama            
    ===============================
    
    tt2850386
    The Croods: A New Age
    330.
    2020
    95 min
    6.9
    43,892
    Animation, Adventure, Comedy            
    ===============================
    
    tt5691670
    Under the Silver Lake
    331.
    2018
    139 min
    6.5
    43,571
    Crime, Drama, Mystery            
    ===============================
    
    tt5117670
    Peter Rabbit
    332.
    2018
    95 min
    6.6
    43,560
    Adventure, Comedy, Crime            
    ===============================
    
    tt5208252
    Operation Finale
    333.
    2018
    122 min
    6.6
    43,186
    Biography, Drama, History            
    ===============================
    
    tt6772802
    Hillbilly Elegy
    334.
    2020
    116 min
    6.7
    42,770
    Drama            
    ===============================
    
    tt8508734
    His House
    335.
    2020
    93 min
    6.5
    42,763
    Drama, Horror, Thriller            
    ===============================
    
    tt2235695
    Rebecca
    336.
    2020
    123 min
    6.0
    42,616
    Drama, Mystery, Romance            
    ===============================
    
    tt5563334
    The Good Liar
    337.
    2019
    109 min
    6.7
    42,604
    Crime, Drama, Mystery            
    ===============================
    
    tt7985648
    The Legend of Tomiris
    338.
    2019
    156 min
    6.1
    42,330
    Action, Drama, History            
    ===============================
    
    tt6476140
    Serenity
    339.
    2019
    106 min
    5.4
    41,865
    Drama, Mystery, Thriller            
    ===============================
    
    tt0805647
    The Witches
    340.
    2020
    106 min
    5.3
    41,785
    Adventure, Comedy, Family            
    ===============================
    
    tt5774450
    Summer of 84
    341.
    2018
    105 min
    6.7
    41,758
    Drama, Horror, Mystery            
    ===============================
    
    tt9426210
    Weathering with You
    342.
    2019
    112 min
    7.5
    41,735
    Animation, Drama, Fantasy            
    ===============================
    
    tt3361792
    Tolkien
    343.
    2019
    112 min
    6.8
    41,601
    Biography, Drama, Romance            
    ===============================
    
    tt6781982
    Night School
    344.
    2018
    111 min
    5.6
    41,411
    Comedy            
    ===============================
    
    tt5001754
    Braven
    345.
    2018
    94 min
    5.9
    41,224
    Action, Thriller            
    ===============================
    
    tt11024272
    The Babysitter: Killer Queen
    346.
    2020
    101 min
    5.8
    41,169
    Comedy, Horror            
    ===============================
    
    tt8206668
    Bad Education
    347.
    2019
    108 min
    7.1
    40,668
    Biography, Comedy, Crime            
    ===============================
    
    tt6259380
    Code 8
    348.
    2019
    98 min
    6.1
    40,516
    Action, Crime, Drama            
    ===============================
    
    tt9619798
    The Wrong Missy
    349.
    2020
    90 min
    5.7
    40,329
    Comedy, Romance            
    ===============================
    
    tt10620868
    #Alive
    350.
    2020
    98 min
    6.3
    40,291
    Action, Drama, Horror            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=351&title_type=feature&year=2018,2020
    tt1620981
    The Addams Family
    351.
    2019
    86 min
    5.8
    40,282
    Animation, Comedy, Family            
    ===============================
    
    tt8522006
    Happiest Season
    352.
    2020
    102 min
    6.6
    40,156
    Comedy, Drama, Romance            
    ===============================
    
    tt2395469
    Gully Boy
    353.
    2019
    154 min
    7.9
    40,122
    Drama, Music, Romance            
    ===============================
    
    tt6182908
    Smallfoot
    354.
    2018
    96 min
    6.6
    40,022
    Animation, Adventure, Comedy            
    ===============================
    
    tt6803046
    The Vast of Night
    355.
    2019
    91 min
    6.7
    40,012
    Drama, Mystery, Sci-Fi            
    ===============================
    
    tt7212754
    Ludo
    356.
    2020
    150 min
    7.6
    39,911
    Action, Comedy, Crime            
    ===============================
    
    tt9354842
    To All the Boys: P.S. I Still Love You
    357.
    2020
    101 min
    6.0
    39,754
    Comedy, Drama, Romance            
    ===============================
    
    tt8201170
    The Perfect Date
    358.
    2019
    89 min
    5.8
    39,730
    Comedy, Romance            
    ===============================
    
    tt3120280
    Sierra Burgess Is a Loser
    359.
    2018
    105 min
    5.8
    39,404
    Comedy, Drama, Romance            
    ===============================
    
    tt7008872
    Boy Erased
    360.
    2018
    115 min
    6.9
    39,230
    Biography, Drama            
    ===============================
    
    tt10039344
    Countdown
    361.
    2019
    90 min
    5.4
    39,220
    Horror, Thriller            
    ===============================
    
    tt4154916
    Replicas
    362.
    2018
    107 min
    5.4
    39,023
    Drama, Sci-Fi, Thriller            
    ===============================
    
    tt1563742
    Overboard
    363.
    2018
    112 min
    6.0
    38,978
    Comedy, Romance            
    ===============================
    
    tt6324278
    Abominable
    364.
    2019
    97 min
    7.0
    38,927
    Animation, Adventure, Comedy            
    ===============================
    
    tt7533152
    The Boy Who Harnessed the Wind
    365.
    2019
    113 min
    7.6
    38,883
    Biography, Drama, History            
    ===============================
    
    tt7557108
    Saint Maud
    366.
    2019
    84 min
    6.7
    38,310
    Drama, Horror, Mystery            
    ===============================
    
    tt4537896
    White Boy Rick
    367.
    2018
    111 min
    6.5
    38,306
    Crime, Drama            
    ===============================
    
    tt9683478
    The Half of It
    368.
    2020
    104 min
    6.9
    38,303
    Comedy, Drama, Romance            
    ===============================
    
    tt6107548
    Late Night
    369.
    2019
    102 min
    6.5
    38,121
    Comedy, Drama            
    ===============================
    
    tt7339248
    The Art of Self-Defense
    370.
    2019
    104 min
    6.6
    38,087
    Action, Comedy, Crime            
    ===============================
    
    tt5797184
    Escape from Pretoria
    371.
    2020
    106 min
    6.8
    37,913
    Biography, Crime, Mystery            
    ===============================
    
    tt8151874
    Honey Boy
    372.
    2019
    94 min
    7.2
    37,908
    Drama            
    ===============================
    
    tt1833116
    The Informer
    373.
    2019
    113 min
    6.6
    37,854
    Action, Crime, Drama            
    ===============================
    
    tt3385524
    Stan & Ollie
    374.
    2018
    98 min
    7.2
    37,802
    Biography, Comedy, Drama            
    ===============================
    
    tt5057140
    Hold the Dark
    375.
    2018
    125 min
    5.6
    37,761
    Action, Drama, Thriller            
    ===============================
    
    tt5619332
    Life of the Party
    376.
    2018
    105 min
    5.6
    37,575
    Comedy            
    ===============================
    
    tt1255919
    Holmes & Watson
    377.
    2018
    90 min
    3.9
    36,994
    Comedy, Crime, Mystery            
    ===============================
    
    tt5918982
    Possessor
    378.
    2020
    103 min
    6.5
    36,975
    Horror, Mystery, Sci-Fi            
    ===============================
    
    tt6697582
    Arif V 216
    379.
    2018
    125 min
    7.0
    36,943
    Comedy, Fantasy, Music            
    ===============================
    
    tt5580266
    The Hate U Give
    380.
    2018
    133 min
    7.5
    36,834
    Crime, Drama            
    ===============================
    
    tt5294518
    Eli
    381.
    2019
    98 min
    5.8
    36,714
    Drama, Horror, Mystery            
    ===============================
    
    tt4827558
    High Life
    382.
    2018
    113 min
    5.8
    36,488
    Adventure, Drama, Horror            
    ===============================
    
    tt8781414
    Freaks
    383.
    2018
    105 min
    6.7
    36,019
    Drama, Mystery, Sci-Fi            
    ===============================
    
    tt1464763
    Mute
    384.
    2018
    126 min
    5.4
    35,869
    Mystery, Sci-Fi, Thriller            
    ===============================
    
    tt6938828
    At Eternity's Gate
    385.
    2018
    111 min
    6.9
    35,691
    Biography, Drama, History            
    ===============================
    
    tt3833480
    The Outpost
    386.
    2019
    123 min
    6.8
    35,201
    Action, Drama, History            
    ===============================
    
    tt4964788
    Everybody Knows
    387.
    2018
    133 min
    6.9
    35,183
    Crime, Drama, Mystery            
    ===============================
    
    tt7725596
    Badhaai Ho
    388.
    2018
    124 min
    7.9
    35,017
    Comedy, Drama            
    ===============================
    
    tt4669788
    On the Basis of Sex
    389.
    2018
    120 min
    7.1
    34,969
    Biography, Drama            
    ===============================
    
    tt10350626
    Gunjan Saxena: The Kargil Girl
    390.
    2020
    112 min
    5.6
    34,907
    Action, Biography, Drama            
    ===============================
    
    tt5690360
    Slender Man
    391.
    2018
    93 min
    3.2
    34,888
    Horror, Mystery, Thriller            
    ===============================
    
    tt7608028
    The Open House
    392.
    2018
    94 min
    3.3
    34,873
    Horror, Thriller            
    ===============================
    
    tt3256226
    IO
    393.
    2019
    96 min
    4.7
    34,675
    Adventure, Drama, Sci-Fi            
    ===============================
    
    tt1072748
    Winchester
    394.
    2018
    99 min
    5.4
    34,670
    Drama, Fantasy, Horror            
    ===============================
    
    tt8108202
    Stree
    395.
    2018
    128 min
    7.5
    34,529
    Comedy, Horror            
    ===============================
    
    tt4878482
    Dumplin'
    396.
    2018
    110 min
    6.5
    34,483
    Comedy, Drama            
    ===============================
    
    tt7280898
    22 July
    397.
    2018
    143 min
    6.8
    34,478
    Crime, Drama, History            
    ===============================
    
    tt2639336
    Greta
    398.
    2018
    98 min
    6.0
    34,429
    Drama, Mystery, Thriller            
    ===============================
    
    tt8108268
    The Tashkent Files
    399.
    2019
    134 min
    8.1
    34,317
    Drama, Mystery, Thriller            
    ===============================
    
    tt10324144
    Article 15
    400.
    2019
    130 min
    8.1
    34,050
    Crime, Drama, Mystery            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=401&title_type=feature&year=2018,2020
    tt6141246
    The Aeronauts
    401.
    2019
    100 min
    6.6
    34,028
    Action, Adventure, Drama            
    ===============================
    
    tt6802308
    The 15:17 to Paris
    402.
    2018
    94 min
    5.3
    34,014
    Biography, Drama, Thriller            
    ===============================
    
    tt8983202
    Kabir Singh
    403.
    2019
    173 min
    7.0
    33,918
    Action, Drama, Romance            
    ===============================
    
    tt5523010
    The Nutcracker and the Four Realms
    404.
    2018
    99 min
    5.5
    33,912
    Adventure, Family, Fantasy            
    ===============================
    
    tt4761916
    Unfriended: Dark Web
    405.
    2018
    92 min
    6.0
    33,839
    Horror, Mystery, Thriller            
    ===============================
    
    tt8633462
    Quo Vadis, Aida?
    406.
    2020
    101 min
    8.0
    33,815
    Drama, History, War            
    ===============================
    
    tt10530176
    The Call
    407.
    2020
    112 min
    7.1
    33,611
    Crime, Horror, Mystery            
    ===============================
    
    tt6195094
    Incident in a Ghostland
    408.
    2018
    91 min
    6.4
    33,447
    Drama, Horror, Mystery            
    ===============================
    
    tt9806192
    I Lost My Body
    409.
    2019
    81 min
    7.5
    33,397
    Animation, Drama, Fantasy            
    ===============================
    
    tt8850222
    Peninsula
    410.
    2020
    116 min
    5.5
    33,347
    Action, Horror, Thriller            
    ===============================
    
    tt5935704
    Padmaavat
    411.
    2018
    164 min
    7.0
    33,226
    Drama, History, Romance            
    ===============================
    
    tt6865690
    The Professor
    412.
    2018
    90 min
    6.7
    33,218
    Comedy, Drama            
    ===============================
    
    tt9398640
    Between Two Ferns: The Movie
    413.
    2019
    82 min
    6.1
    33,205
    Comedy            
    ===============================
    
    tt2076298
    Ip Man 4: The Finale
    414.
    2019
    107 min
    7.0
    32,815
    Action, Biography, Drama            
    ===============================
    
    tt10003008
    The Rental
    415.
    2020
    88 min
    5.7
    32,793
    Drama, Horror, Mystery            
    ===============================
    
    tt7485048
    Super 30
    416.
    2019
    154 min
    7.9
    32,769
    Biography, Drama            
    ===============================
    
    tt4073790
    The Darkest Minds
    417.
    2018
    104 min
    5.7
    32,745
    Action, Adventure, Drama            
    ===============================
    
    tt5198068
    Wolfwalkers
    418.
    2020
    103 min
    8.0
    32,741
    Animation, Adventure, Family            
    ===============================
    
    tt8108274
    Tanhaji: The Unsung Warrior
    419.
    2020
    135 min
    7.5
    32,685
    Action, Biography, Drama            
    ===============================
    
    tt9606374
    On the Rocks
    420.
    2020
    96 min
    6.4
    32,652
    Comedy, Drama, Romance            
    ===============================
    
    tt8361028
    Cam
    421.
    2018
    94 min
    5.9
    32,624
    Drama, Horror, Mystery            
    ===============================
    
    tt9900782
    Kaithi
    422.
    2019
    145 min
    8.5
    32,569
    Action, Adventure, Crime            
    ===============================
    
    tt10362466
    After We Collided
    423.
    2020
    105 min
    5.0
    32,474
    Drama, Romance            
    ===============================
    
    tt9784456
    The Kissing Booth 2
    424.
    2020
    134 min
    5.7
    32,438
    Comedy, Romance            
    ===============================
    
    tt6513656
    Escape Plan 2: Hades
    425.
    2018
    96 min
    3.8
    32,411
    Action, Crime, Mystery            
    ===============================
    
    tt3907584
    All the Bright Places
    426.
    2020
    107 min
    6.5
    32,326
    Drama, Romance            
    ===============================
    
    tt10612922
    One Night in Miami...
    427.
    2020
    114 min
    7.1
    32,298
    Drama            
    ===============================
    
    tt1478839
    The Art of Racing in the Rain
    428.
    2019
    109 min
    7.5
    32,207
    Comedy, Drama, Romance            
    ===============================
    
    tt8722346
    Queen & Slim
    429.
    2019
    132 min
    7.1
    32,070
    Crime, Drama, Romance            
    ===============================
    
    tt4799066
    Midnight Sun
    430.
    2018
    91 min
    6.6
    32,018
    Drama, Romance            
    ===============================
    
    tt4357394
    Tau
    431.
    2018
    97 min
    5.8
    31,974
    Sci-Fi, Thriller            
    ===============================
    
    tt9016974
    Synchronic
    432.
    2019
    102 min
    6.2
    31,844
    Crime, Drama, Horror            
    ===============================
    
    tt1285009
    The Strangers: Prey at Night
    433.
    2018
    85 min
    5.2
    31,777
    Horror            
    ===============================
    
    tt4986098
    The Titan
    434.
    2018
    97 min
    4.8
    31,725
    Drama, Mystery, Romance            
    ===============================
    
    tt7019842
    96
    435.
    2018
    158 min
    8.5
    31,611
    Drama, Romance            
    ===============================
    
    tt4068576
    The Nightingale
    436.
    2018
    136 min
    7.3
    31,567
    Adventure, Drama, Thriller            
    ===============================
    
    tt9086228
    Gretel & Hansel
    437.
    2020
    87 min
    5.5
    31,384
    Fantasy, Horror, Mystery            
    ===============================
    
    tt7547410
    Dora and the Lost City of Gold
    438.
    2019
    102 min
    6.1
    31,381
    Action, Adventure, Comedy            
    ===============================
    
    tt7098658
    Raazi
    439.
    2018
    138 min
    7.7
    31,359
    Action, Drama, Thriller            
    ===============================
    
    tt7721800
    Bharat
    440.
    2019
    150 min
    4.7
    31,275
    Action, Comedy, Drama            
    ===============================
    
    tt7242142
    Blindspotting
    441.
    2018
    95 min
    7.4
    31,158
    Comedy, Crime, Drama            
    ===============================
    
    tt8242084
    My Spy
    442.
    2020
    99 min
    6.3
    31,150
    Action, Comedy            
    ===============================
    
    tt12749596
    Host
    443.
    2020
    57 min
    6.5
    31,071
    Horror, Mystery            
    ===============================
    
    tt9196192
    Cuties
    444.
    2020
    96 min
    3.5
    30,906
    Drama            
    ===============================
    
    tt7605074
    The Wandering Earth
    445.
    2019
    125 min
    5.9
    30,891
    Action, Adventure, Sci-Fi            
    ===============================
    
    tt3317234
    Game Over, Man!
    446.
    2018
    101 min
    5.4
    30,880
    Action, Comedy            
    ===============================
    
    tt5501104
    Border
    447.
    2018
    110 min
    7.0
    30,648
    Crime, Drama, Fantasy            
    ===============================
    
    tt3721964
    Gringo
    448.
    2018
    111 min
    6.1
    30,595
    Action, Comedy, Crime            
    ===============================
    
    tt6987770
    Destination Wedding
    449.
    2018
    87 min
    6.0
    30,524
    Comedy, Drama, Romance            
    ===============================
    
    tt5867314
    The Empty Man
    450.
    2020
    137 min
    6.2
    30,464
    Horror, Mystery, Thriller            
    ===============================
    
    http://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=451&title_type=feature&year=2018,2020
    tt11057644
    The Christmas Chronicles: Part Two
    451.
    2020
    112 min
    6.0
    30,456
    Adventure, Comedy, Family            
    ===============================
    
    tt10521092
    The Forgotten Battle
    452.
    2020
    124 min
    7.1
    30,270
    Drama, War            
    ===============================
    
    tt6095472
    The Angry Birds Movie 2
    453.
    2019
    97 min
    6.4
    30,205
    Animation, Action, Adventure            
    ===============================
    
    tt6535880
    Haunt
    454.
    2019
    92 min
    6.3
    30,070
    Horror, Thriller            
    ===============================
    
    tt7137380
    Destroyer
    455.
    2018
    121 min
    6.2
    30,022
    Action, Crime, Drama            
    ===============================
    
    tt6218358
    Calibre
    456.
    2018
    101 min
    6.8
    29,935
    Drama, Thriller            
    ===============================
    
    tt4995776
    The Red Sea Diving Resort
    457.
    2019
    129 min
    6.6
    29,840
    Drama, History, Thriller            
    ===============================
    
    tt9477520
    Asuran
    458.
    2019
    141 min
    8.4
    29,788
    Action, Drama            
    ===============================
    
    tt6436726
    7500
    459.
    2019
    93 min
    6.3
    29,562
    Action, Drama, Thriller            
    ===============================
    
    tt5929754
    Wildlife
    460.
    2018
    105 min
    6.8
    29,373
    Drama            
    ===============================
    
    tt6285944
    The Banker
    461.
    2020
    120 min
    7.3
    29,107
    Biography, Drama            
    ===============================
    
    tt10964430
    Thappad
    462.
    2020
    142 min
    7.0
    28,756
    Drama            
    ===============================
    
    tt3089630
    Artemis Fowl
    463.
    2020
    95 min
    4.2
    28,620
    Adventure, Family, Fantasy            
    ===============================
    
    tt6857166
    Book Club
    464.
    2018
    104 min
    6.1
    28,608
    Comedy, Drama, Romance            
    ===============================
    
    tt7772582
    Never Rarely Sometimes Always
    465.
    2020
    101 min
    7.4
    28,606
    Drama            
    ===============================
    
    tt8954732
    The Princess Switch
    466.
    2018
    101 min
    6.0
    28,559
    Comedy, Drama, Family            
    ===============================
    
    tt6269368
    The Clovehitch Killer
    467.
    2018
    109 min
    6.5
    28,542
    Crime, Drama, Mystery            
    ===============================
    
    tt1308728
    The Happytime Murders
    468.
    2018
    91 min
    5.4
    28,517
    Action, Comedy, Crime            
    ===============================
    
    tt8130968
    Badla
    469.
    2019
    118 min
    7.8
    28,494
    Crime, Drama, Mystery            
    ===============================
    
    tt7946422
    Prospect
    470.
    2018
    100 min
    6.3
    28,387
    Adventure, Drama, Sci-Fi            
    ===============================
    
    tt7634968
    What Men Want
    471.
    2019
    117 min
    5.3
    28,344
    Comedy, Fantasy, Romance            
    ===============================
    
    tt7961060
    Dragon Ball Super: Broly
    472.
    2018
    100 min
    7.7
    28,218
    Animation, Action, Adventure            
    ===============================
    
    tt6527426
    Zero
    473.
    2018
    164 min
    5.2
    28,114
    Comedy, Drama, Romance            
    ===============================
    
    tt2126357
    Second Act
    474.
    2018
    103 min
    5.8
    28,000
    Comedy, Drama, Romance            
    ===============================
    
    tt9354944
    Jexi
    475.
    2019
    84 min
    6.0
    27,910
    Comedy, Romance, Sci-Fi            
    ===============================
    
    tt2762506
    Bacurau
    476.
    2019
    131 min
    7.3
    27,891
    Action, Adventure, Horror            
    ===============================
    
    tt6288124
    Don't Worry, He Won't Get Far on Foot
    477.
    2018
    114 min
    6.8
    27,588
    Biography, Comedy, Drama            
    ===============================
    
    tt10161886
    The Prom
    478.
    2020
    130 min
    5.9
    27,523
    Comedy, Drama, Musical            
    ===============================
    
    tt4648786
    Harriet
    479.
    2019
    125 min
    6.7
    27,477
    Action, Biography, Drama            
    ===============================
    
    tt10065694
    Antebellum
    480.
    2020
    105 min
    5.8
    27,429
    Drama, Horror, Mystery            
    ===============================
    
    tt9691136
    Shadow in the Cloud
    481.
    2020
    83 min
    5.0
    27,364
    Action, Horror, War            
    ===============================
    
    tt6768578
    Dogman
    482.
    2018
    103 min
    7.2
    27,280
    Crime, Drama, Thriller            
    ===============================
    
    tt9072352
    Relic
    483.
    2020
    89 min
    6.0
    27,276
    Drama, Horror, Mystery            
    ===============================
    
    tt9252468
    Mosul
    484.
    2019
    101 min
    7.1
    27,252
    Action, Drama, War            
    ===============================
    
    tt5144174
    The Dry
    485.
    2020
    117 min
    6.8
    27,209
    Crime, Drama, Mystery            
    ===============================
    
    tt9614460
    Chhapaak
    486.
    2020
    120 min
    5.3
    27,207
    Biography, Drama            
    ===============================
    
    tt7748244
    Mortal World
    487.
    2018
    107 min
    7.6
    27,186
    Action, Comedy, Crime            
    ===============================
    
    tt3152592
    Scoob!
    488.
    2020
    93 min
    5.6
    27,170
    Animation, Adventure, Comedy            
    ===============================
    
    tt9340860
    Let Him Go
    489.
    2020
    113 min
    6.7
    27,145
    Crime, Drama, Thriller            
    ===============================
    
    tt7329656
    47 Meters Down: Uncaged
    490.
    2019
    90 min
    5.0
    26,919
    Adventure, Drama, Horror            
    ===============================
    
    tt0859635
    Super Troopers 2
    491.
    2018
    99 min
    6.0
    26,897
    Comedy, Crime, Mystery            
    ===============================
    
    tt4504044
    The Prodigy
    492.
    2019
    92 min
    5.9
    26,802
    Fantasy, Horror, Mystery            
    ===============================
    
    tt3612126
    The Grudge
    493.
    2020
    94 min
    4.4
    26,680
    Fantasy, Horror            
    ===============================
    
    tt7218518
    Pad Man
    494.
    2018
    140 min
    7.9
    26,569
    Biography, Comedy, Drama            
    ===============================
    
    tt6116856
    The Night Comes for Us
    495.
    2018
    121 min
    6.9
    26,553
    Action, Thriller            
    ===============================
    
    tt6348138
    Missing Link
    496.
    2019
    93 min
    6.7
    26,508
    Animation, Adventure, Comedy            
    ===============================
    
    tt2011311
    The Outsider
    497.
    2018
    120 min
    6.2
    26,458
    Action, Crime, Drama            
    ===============================
    
    tt8851668
    The Lovebirds
    498.
    2020
    86 min
    6.1
    26,372
    Action, Comedy, Crime            
    ===============================
    
    tt6521876
    Jay and Silent Bob Reboot
    499.
    2019
    105 min
    5.6
    26,148
    Action, Adventure, Comedy            
    ===============================
    
    tt8652728
    Waves
    500.
    2019
    135 min
    7.5
    26,128
    Drama, Romance, Sport            
    ===============================
    
    [{'movie_id': 'tt7286456', 'title': 'Joker', 'rank': '1.', 'year': '2019', 'runtime': '122 min', 'rating': '8.4', 'votes': '1,271,181', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt4154796', 'title': 'Avengers: Endgame', 'rank': '2.', 'year': '2019', 'runtime': '181 min', 'rating': '8.4', 'votes': '1,121,459', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt4154756', 'title': 'Avengers: Infinity War', 'rank': '3.', 'year': '2018', 'runtime': '149 min', 'rating': '8.4', 'votes': '1,073,326', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt6751668', 'title': 'Parasite', 'rank': '4.', 'year': '2019', 'runtime': '132 min', 'rating': '8.5', 'votes': '794,414', 'genres': 'Drama, Thriller            '}, {'movie_id': 'tt1825683', 'title': 'Black Panther', 'rank': '5.', 'year': '2018', 'runtime': '134 min', 'rating': '7.3', 'votes': '767,098', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt7131622', 'title': 'Once Upon a Time in Hollywood', 'rank': '6.', 'year': '2019', 'runtime': '161 min', 'rating': '7.6', 'votes': '731,841', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt8946378', 'title': 'Knives Out', 'rank': '7.', 'year': '2019', 'runtime': '130 min', 'rating': '7.9', 'votes': '625,540', 'genres': 'Comedy, Crime, Drama            '}, {'movie_id': 'tt8579674', 'title': '1917', 'rank': '8.', 'year': '2019', 'runtime': '119 min', 'rating': '8.2', 'votes': '585,340', 'genres': 'Action, Drama, War            '}, {'movie_id': 'tt5463162', 'title': 'Deadpool 2', 'rank': '9.', 'year': '2018', 'runtime': '119 min', 'rating': '7.7', 'votes': '581,115', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt4154664', 'title': 'Captain Marvel', 'rank': '10.', 'year': '2019', 'runtime': '123 min', 'rating': '6.8', 'votes': '559,308', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt1727824', 'title': 'Bohemian Rhapsody', 'rank': '11.', 'year': '2018', 'runtime': '134 min', 'rating': '7.9', 'votes': '537,886', 'genres': 'Biography, Drama, Music            '}, {'movie_id': 'tt6644200', 'title': 'A Quiet Place', 'rank': '12.', 'year': '2018', 'runtime': '90 min', 'rating': '7.5', 'votes': '528,078', 'genres': 'Drama, Horror, Sci-Fi            '}, {'movie_id': 'tt4633694', 'title': 'Spider-Man: Into the Spider-Verse', 'rank': '13.', 'year': '2018', 'runtime': '117 min', 'rating': '8.4', 'votes': '524,990', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt6723592', 'title': 'Tenet', 'rank': '14.', 'year': '2020', 'runtime': '150 min', 'rating': '7.3', 'votes': '501,142', 'genres': 'Action, Sci-Fi, Thriller            '}, {'movie_id': 'tt6320628', 'title': 'Spider-Man: Far from Home', 'rank': '15.', 'year': '2019', 'runtime': '129 min', 'rating': '7.4', 'votes': '490,791', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt6966692', 'title': 'Green Book', 'rank': '16.', 'year': '2018', 'runtime': '130 min', 'rating': '8.2', 'votes': '489,304', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt1270797', 'title': 'Venom', 'rank': '17.', 'year': '2018', 'runtime': '112 min', 'rating': '6.6', 'votes': '484,803', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt1477834', 'title': 'Aquaman', 'rank': '18.', 'year': '2018', 'runtime': '143 min', 'rating': '6.8', 'votes': '474,104', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt2527338', 'title': 'Star Wars: The Rise Of Skywalker', 'rank': '19.', 'year': '2019', 'runtime': '141 min', 'rating': '6.5', 'votes': '450,328', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt1677720', 'title': 'Ready Player One', 'rank': '20.', 'year': '2018', 'runtime': '140 min', 'rating': '7.4', 'votes': '436,280', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt5095030', 'title': 'Ant-Man and the Wasp', 'rank': '21.', 'year': '2018', 'runtime': '118 min', 'rating': '7.0', 'votes': '401,934', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt1950186', 'title': 'Ford v Ferrari', 'rank': '22.', 'year': '2019', 'runtime': '152 min', 'rating': '8.1', 'votes': '392,046', 'genres': 'Action, Biography, Drama            '}, {'movie_id': 'tt2584384', 'title': 'Jojo Rabbit', 'rank': '23.', 'year': '2019', 'runtime': '108 min', 'rating': '7.9', 'votes': '388,254', 'genres': 'Comedy, Drama, War            '}, {'movie_id': 'tt1302006', 'title': 'The Irishman', 'rank': '24.', 'year': '2019', 'runtime': '209 min', 'rating': '7.8', 'votes': '387,450', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt1517451', 'title': 'A Star Is Born', 'rank': '25.', 'year': '2018', 'runtime': '136 min', 'rating': '7.6', 'votes': '384,165', 'genres': 'Drama, Music, Romance            '}, {'movie_id': 'tt3778644', 'title': 'Solo: A Star Wars Story', 'rank': '26.', 'year': '2018', 'runtime': '135 min', 'rating': '6.9', 'votes': '347,374', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt6146586', 'title': 'John Wick: Chapter 3 - Parabellum', 'rank': '27.', 'year': '2019', 'runtime': '130 min', 'rating': '7.4', 'votes': '343,721', 'genres': 'Action, Crime, Thriller            '}, {'movie_id': 'tt2737304', 'title': 'Bird Box', 'rank': '28.', 'year': '2018', 'runtime': '124 min', 'rating': '6.6', 'votes': '342,273', 'genres': 'Horror, Mystery, Sci-Fi            '}, {'movie_id': 'tt8367814', 'title': 'The Gentlemen', 'rank': '29.', 'year': '2019', 'runtime': '113 min', 'rating': '7.8', 'votes': '338,310', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt0448115', 'title': 'Shazam!', 'rank': '30.', 'year': '2019', 'runtime': '132 min', 'rating': '7.0', 'votes': '338,065', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt4912910', 'title': 'Mission: Impossible - Fallout', 'rank': '31.', 'year': '2018', 'runtime': '147 min', 'rating': '7.7', 'votes': '333,151', 'genres': 'Action, Adventure, Thriller            '}, {'movie_id': 'tt2948372', 'title': 'Soul', 'rank': '32.', 'year': '2020', 'runtime': '100 min', 'rating': '8.0', 'votes': '328,563', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt2798920', 'title': 'Annihilation', 'rank': '33.', 'year': '2018', 'runtime': '115 min', 'rating': '6.8', 'votes': '327,214', 'genres': 'Adventure, Drama, Horror            '}, {'movie_id': 'tt8772262', 'title': 'Midsommar', 'rank': '34.', 'year': '2019', 'runtime': '148 min', 'rating': '7.1', 'votes': '321,719', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt4881806', 'title': 'Jurassic World: Fallen Kingdom', 'rank': '35.', 'year': '2018', 'runtime': '128 min', 'rating': '6.1', 'votes': '317,221', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt7784604', 'title': 'Hereditary', 'rank': '36.', 'year': '2018', 'runtime': '127 min', 'rating': '7.3', 'votes': '316,203', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt7653254', 'title': 'Marriage Story', 'rank': '37.', 'year': '2019', 'runtime': '137 min', 'rating': '7.9', 'votes': '309,090', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt6857112', 'title': 'Us', 'rank': '38.', 'year': '2019', 'runtime': '116 min', 'rating': '6.8', 'votes': '299,452', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt3606756', 'title': 'Incredibles 2', 'rank': '39.', 'year': '2018', 'runtime': '118 min', 'rating': '7.6', 'votes': '297,461', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt4123430', 'title': 'Fantastic Beasts: The Crimes of Grindelwald', 'rank': '40.', 'year': '2018', 'runtime': '134 min', 'rating': '6.5', 'votes': '283,597', 'genres': 'Adventure, Family, Fantasy            '}, {'movie_id': 'tt5727208', 'title': 'Uncut Gems', 'rank': '41.', 'year': '2019', 'runtime': '135 min', 'rating': '7.4', 'votes': '282,052', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt0437086', 'title': 'Alita: Battle Angel', 'rank': '42.', 'year': '2019', 'runtime': '122 min', 'rating': '7.3', 'votes': '268,520', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt7126948', 'title': 'Wonder Woman 1984', 'rank': '43.', 'year': '2020', 'runtime': '151 min', 'rating': '5.4', 'votes': '267,271', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt7349950', 'title': 'It Chapter Two', 'rank': '44.', 'year': '2019', 'runtime': '169 min', 'rating': '6.5', 'votes': '266,323', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt6139732', 'title': 'Aladdin', 'rank': '45.', 'year': '2019', 'runtime': '128 min', 'rating': '6.9', 'votes': '266,237', 'genres': 'Adventure, Comedy, Family            '}, {'movie_id': 'tt7349662', 'title': 'BlacKkKlansman', 'rank': '46.', 'year': '2018', 'runtime': '135 min', 'rating': '7.5', 'votes': '264,598', 'genres': 'Biography, Comedy, Crime            '}, {'movie_id': 'tt1979376', 'title': 'Toy Story 4', 'rank': '47.', 'year': '2019', 'runtime': '100 min', 'rating': '7.7', 'votes': '250,943', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt7975244', 'title': 'Jumanji: The Next Level', 'rank': '48.', 'year': '2019', 'runtime': '123 min', 'rating': '6.7', 'votes': '250,937', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt9243946', 'title': 'El Camino: A Breaking Bad Movie', 'rank': '49.', 'year': '2019', 'runtime': '122 min', 'rating': '7.3', 'votes': '250,282', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt6105098', 'title': 'The Lion King', 'rank': '50.', 'year': '2019', 'runtime': '118 min', 'rating': '6.8', 'votes': '245,589', 'genres': 'Animation, Adventure, Drama            '}, {'movie_id': 'tt6823368', 'title': 'Glass', 'rank': '51.', 'year': '2019', 'runtime': '129 min', 'rating': '6.6', 'votes': '244,288', 'genres': 'Drama, Horror, Sci-Fi            '}, {'movie_id': 'tt7713068', 'title': 'Birds of Prey', 'rank': '52.', 'year': '2020', 'runtime': '109 min', 'rating': '6.0', 'votes': '239,392', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt2935510', 'title': 'Ad Astra', 'rank': '53.', 'year': '2019', 'runtime': '123 min', 'rating': '6.5', 'votes': '236,215', 'genres': 'Adventure, Drama, Mystery            '}, {'movie_id': 'tt2704998', 'title': 'Game Night', 'rank': '54.', 'year': '2018', 'runtime': '100 min', 'rating': '6.9', 'votes': '235,915', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt1051906', 'title': 'The Invisible Man', 'rank': '55.', 'year': '2020', 'runtime': '124 min', 'rating': '7.1', 'votes': '228,905', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt8332922', 'title': 'A Quiet Place Part II', 'rank': '56.', 'year': '2020', 'runtime': '97 min', 'rating': '7.2', 'votes': '227,454', 'genres': 'Drama, Horror, Sci-Fi            '}, {'movie_id': 'tt8228288', 'title': 'The Platform', 'rank': '57.', 'year': '2019', 'runtime': '94 min', 'rating': '7.0', 'votes': '225,447', 'genres': 'Horror, Sci-Fi, Thriller            '}, {'movie_id': 'tt5164214', 'title': "Ocean's Eight", 'rank': '58.', 'year': '2018', 'runtime': '110 min', 'rating': '6.3', 'votes': '220,331', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt7984734', 'title': 'The Lighthouse', 'rank': '59.', 'year': '2019', 'runtime': '109 min', 'rating': '7.4', 'votes': '217,503', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt1365519', 'title': 'Tomb Raider', 'rank': '60.', 'year': '2018', 'runtime': '119 min', 'rating': '6.3', 'votes': '215,365', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt6806448', 'title': 'Fast & Furious Presents: Hobbs & Shaw', 'rank': '61.', 'year': '2019', 'runtime': '137 min', 'rating': '6.5', 'votes': '213,454', 'genres': 'Action, Adventure, Thriller            '}, {'movie_id': 'tt3281548', 'title': 'Little Women', 'rank': '62.', 'year': '2019', 'runtime': '135 min', 'rating': '7.8', 'votes': '204,997', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt8936646', 'title': 'Extraction', 'rank': '63.', 'year': '2020', 'runtime': '116 min', 'rating': '6.7', 'votes': '203,603', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt5083738', 'title': 'The Favourite', 'rank': '64.', 'year': '2018', 'runtime': '119 min', 'rating': '7.5', 'votes': '201,239', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt5606664', 'title': 'Doctor Sleep', 'rank': '65.', 'year': '2019', 'runtime': '152 min', 'rating': '7.3', 'votes': '194,049', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt7846844', 'title': 'Enola Holmes', 'rank': '66.', 'year': '2020', 'runtime': '123 min', 'rating': '6.6', 'votes': '189,172', 'genres': 'Action, Adventure, Crime            '}, {'movie_id': 'tt1213641', 'title': 'First Man', 'rank': '67.', 'year': '2018', 'runtime': '141 min', 'rating': '7.3', 'votes': '189,093', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt6565702', 'title': 'X-Men: Dark Phoenix', 'rank': '68.', 'year': '2019', 'runtime': '113 min', 'rating': '5.7', 'votes': '188,176', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt2873282', 'title': 'Red Sparrow', 'rank': '69.', 'year': '2018', 'runtime': '140 min', 'rating': '6.6', 'votes': '187,340', 'genres': 'Action, Drama, Thriller            '}, {'movie_id': 'tt6499752', 'title': 'Upgrade', 'rank': '70.', 'year': '2018', 'runtime': '100 min', 'rating': '7.5', 'votes': '186,788', 'genres': 'Action, Sci-Fi, Thriller            '}, {'movie_id': 'tt3741700', 'title': 'Godzilla: King of the Monsters', 'rank': '71.', 'year': '2019', 'runtime': '132 min', 'rating': '6.0', 'votes': '183,708', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt1560220', 'title': 'Zombieland: Double Tap', 'rank': '72.', 'year': '2019', 'runtime': '99 min', 'rating': '6.7', 'votes': '180,853', 'genres': 'Action, Comedy, Horror            '}, {'movie_id': 'tt6450804', 'title': 'Terminator: Dark Fate', 'rank': '73.', 'year': '2019', 'runtime': '128 min', 'rating': '6.2', 'votes': '178,582', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt1070874', 'title': 'The Trial of the Chicago 7', 'rank': '74.', 'year': '2020', 'runtime': '129 min', 'rating': '7.7', 'votes': '177,721', 'genres': 'Drama, History, Thriller            '}, {'movie_id': 'tt8106534', 'title': '6 Underground', 'rank': '75.', 'year': '2019', 'runtime': '128 min', 'rating': '6.1', 'votes': '177,605', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt2066051', 'title': 'Rocketman', 'rank': '76.', 'year': '2019', 'runtime': '121 min', 'rating': '7.3', 'votes': '175,928', 'genres': 'Biography, Drama, Musical            '}, {'movie_id': 'tt3104988', 'title': 'Crazy Rich Asians', 'rank': '77.', 'year': '2018', 'runtime': '120 min', 'rating': '6.9', 'votes': '173,422', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt9620292', 'title': 'Promising Young Woman', 'rank': '78.', 'year': '2020', 'runtime': '113 min', 'rating': '7.5', 'votes': '173,222', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt4520988', 'title': 'Frozen II', 'rank': '79.', 'year': '2019', 'runtime': '103 min', 'rating': '6.8', 'votes': '173,057', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt4779682', 'title': 'The Meg', 'rank': '80.', 'year': '2018', 'runtime': '113 min', 'rating': '5.6', 'votes': '172,473', 'genres': 'Action, Horror, Sci-Fi            '}, {'movie_id': 'tt5104604', 'title': 'Isle of Dogs', 'rank': '81.', 'year': '2018', 'runtime': '101 min', 'rating': '7.8', 'votes': '169,740', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt2231461', 'title': 'Rampage', 'rank': '82.', 'year': '2018', 'runtime': '107 min', 'rating': '6.1', 'votes': '169,347', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt4701182', 'title': 'Bumblebee', 'rank': '83.', 'year': '2018', 'runtime': '114 min', 'rating': '6.7', 'votes': '166,274', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt7556122', 'title': 'The Old Guard', 'rank': '84.', 'year': '2020', 'runtime': '125 min', 'rating': '6.6', 'votes': '165,726', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt5884052', 'title': 'Pokémon Detective Pikachu', 'rank': '85.', 'year': '2019', 'runtime': '104 min', 'rating': '6.5', 'votes': '164,134', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt7668870', 'title': 'Searching', 'rank': '86.', 'year': '2018', 'runtime': '102 min', 'rating': '7.6', 'votes': '163,657', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt5848272', 'title': 'Ralph Breaks the Internet', 'rank': '87.', 'year': '2018', 'runtime': '112 min', 'rating': '7.0', 'votes': '162,112', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt1502397', 'title': 'Bad Boys for Life', 'rank': '88.', 'year': '2020', 'runtime': '124 min', 'rating': '6.5', 'votes': '161,393', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt9770150', 'title': 'Nomadland', 'rank': '89.', 'year': '2020', 'runtime': '107 min', 'rating': '7.3', 'votes': '161,208', 'genres': 'Drama            '}, {'movie_id': 'tt3766354', 'title': 'The Equalizer 2', 'rank': '90.', 'year': '2018', 'runtime': '121 min', 'rating': '6.7', 'votes': '161,070', 'genres': 'Action, Crime, Thriller            '}, {'movie_id': 'tt9484998', 'title': 'Palm Springs', 'rank': '91.', 'year': '2020', 'runtime': '90 min', 'rating': '7.4', 'votes': '159,737', 'genres': 'Comedy, Fantasy, Mystery            '}, {'movie_id': 'tt6155172', 'title': 'Roma', 'rank': '92.', 'year': '2018', 'runtime': '135 min', 'rating': '7.7', 'votes': '158,974', 'genres': 'Drama            '}, {'movie_id': 'tt10288566', 'title': 'Another Round', 'rank': '93.', 'year': '2020', 'runtime': '117 min', 'rating': '7.7', 'votes': '157,969', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt1502407', 'title': 'Halloween', 'rank': '94.', 'year': '2018', 'runtime': '106 min', 'rating': '6.5', 'votes': '156,320', 'genres': 'Crime, Horror, Thriller            '}, {'movie_id': 'tt8079248', 'title': 'Yesterday', 'rank': '95.', 'year': '2019', 'runtime': '116 min', 'rating': '6.8', 'votes': '153,653', 'genres': 'Comedy, Fantasy, Music            '}, {'movie_id': 'tt10272386', 'title': 'The Father', 'rank': '96.', 'year': '2020', 'runtime': '97 min', 'rating': '8.2', 'votes': '152,850', 'genres': 'Drama, Mystery            '}, {'movie_id': 'tt7798634', 'title': 'Ready or Not', 'rank': '97.', 'year': '2019', 'runtime': '95 min', 'rating': '6.8', 'votes': '152,395', 'genres': 'Action, Comedy, Horror            '}, {'movie_id': 'tt6628394', 'title': 'Bad Times at the El Royale', 'rank': '98.', 'year': '2018', 'runtime': '141 min', 'rating': '7.1', 'votes': '151,350', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt7040874', 'title': 'A Simple Favor', 'rank': '99.', 'year': '2018', 'runtime': '117 min', 'rating': '6.8', 'votes': '150,432', 'genres': 'Comedy, Crime, Mystery            '}, {'movie_id': 'tt4729430', 'title': 'Klaus', 'rank': '100.', 'year': '2019', 'runtime': '96 min', 'rating': '8.1', 'votes': '150,008', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt7146812', 'title': 'Onward', 'rank': '101.', 'year': '2020', 'runtime': '102 min', 'rating': '7.4', 'votes': '149,883', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt5052474', 'title': 'Sicario: Day of the Soldado', 'rank': '102.', 'year': '2018', 'runtime': '122 min', 'rating': '7.0', 'votes': '149,090', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt6412452', 'title': 'The Ballad of Buster Scruggs', 'rank': '103.', 'year': '2018', 'runtime': '133 min', 'rating': '7.3', 'votes': '148,746', 'genres': 'Comedy, Drama, Musical            '}, {'movie_id': 'tt4566758', 'title': 'Mulan', 'rank': '104.', 'year': '2020', 'runtime': '115 min', 'rating': '5.7', 'votes': '148,665', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt6266538', 'title': 'Vice', 'rank': '105.', 'year': '2018', 'runtime': '132 min', 'rating': '7.2', 'votes': '148,538', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt5814060', 'title': 'The Nun', 'rank': '106.', 'year': '2018', 'runtime': '96 min', 'rating': '5.3', 'votes': '142,259', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt4500922', 'title': 'Maze Runner: The Death Cure', 'rank': '107.', 'year': '2018', 'runtime': '143 min', 'rating': '6.2', 'votes': '141,412', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt13143964', 'title': 'Borat Subsequent Moviefilm', 'rank': '108.', 'year': '2020', 'runtime': '95 min', 'rating': '6.6', 'votes': '141,407', 'genres': 'Comedy            '}, {'movie_id': 'tt3794354', 'title': 'Sonic the Hedgehog', 'rank': '109.', 'year': '2020', 'runtime': '99 min', 'rating': '6.5', 'votes': '140,177', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt7959026', 'title': 'The Mule', 'rank': '110.', 'year': '2018', 'runtime': '116 min', 'rating': '7.0', 'votes': '138,359', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt7395114', 'title': 'The Devil All the Time', 'rank': '111.', 'year': '2020', 'runtime': '138 min', 'rating': '7.1', 'votes': '135,582', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt2283336', 'title': 'Men in Black: International', 'rank': '112.', 'year': '2019', 'runtime': '114 min', 'rating': '5.6', 'votes': '135,465', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt2854926', 'title': 'Tag', 'rank': '113.', 'year': '2018', 'runtime': '100 min', 'rating': '6.5', 'votes': '134,919', 'genres': 'Action, Comedy            '}, {'movie_id': 'tt3829266', 'title': 'The Predator', 'rank': '114.', 'year': '2018', 'runtime': '107 min', 'rating': '5.3', 'votes': '134,630', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt9893250', 'title': 'I Care a Lot', 'rank': '115.', 'year': '2020', 'runtime': '118 min', 'rating': '6.3', 'votes': '133,361', 'genres': 'Comedy, Crime, Drama            '}, {'movie_id': 'tt8110330', 'title': 'Dil Bechara', 'rank': '116.', 'year': '2020', 'runtime': '101 min', 'rating': '8.1', 'votes': '131,177', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt7984766', 'title': 'The King', 'rank': '117.', 'year': '2019', 'runtime': '140 min', 'rating': '7.3', 'votes': '130,503', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt2386490', 'title': 'How to Train Your Dragon: The Hidden World', 'rank': '118.', 'year': '2019', 'runtime': '104 min', 'rating': '7.4', 'votes': '129,945', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt1488606', 'title': 'Triple Frontier', 'rank': '119.', 'year': '2019', 'runtime': '125 min', 'rating': '6.4', 'votes': '129,554', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt1618434', 'title': 'Murder Mystery', 'rank': '120.', 'year': '2019', 'runtime': '97 min', 'rating': '6.0', 'votes': '129,227', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt5363618', 'title': 'Sound of Metal', 'rank': '121.', 'year': '2019', 'runtime': '120 min', 'rating': '7.7', 'votes': '128,340', 'genres': 'Drama, Music            '}, {'movie_id': 'tt2222042', 'title': 'Love and Monsters', 'rank': '122.', 'year': '2020', 'runtime': '109 min', 'rating': '6.9', 'votes': '128,171', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt1571234', 'title': 'Mortal Engines', 'rank': '123.', 'year': '2018', 'runtime': '128 min', 'rating': '6.1', 'votes': '127,438', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt9495224', 'title': 'Black Mirror: Bandersnatch', 'rank': '124.', 'year': '2018', 'runtime': '90 min', 'rating': '7.1', 'votes': '127,379', 'genres': 'Drama, Fantasy, Mystery            '}, {'movie_id': 'tt6343314', 'title': 'Creed II', 'rank': '125.', 'year': '2018', 'runtime': '130 min', 'rating': '7.1', 'votes': '126,612', 'genres': 'Drama, Sport            '}, {'movie_id': 'tt8404614', 'title': 'The Two Popes', 'rank': '126.', 'year': '2019', 'runtime': '125 min', 'rating': '7.6', 'votes': '124,359', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt5886046', 'title': 'Escape Room', 'rank': '127.', 'year': '2019', 'runtime': '99 min', 'rating': '6.4', 'votes': '123,696', 'genres': 'Action, Adventure, Horror            '}, {'movie_id': 'tt5758778', 'title': 'Skyscraper', 'rank': '128.', 'year': '2018', 'runtime': '102 min', 'rating': '5.8', 'votes': '123,391', 'genres': 'Action, Adventure, Thriller            '}, {'movie_id': 'tt8847712', 'title': 'The French Dispatch', 'rank': '129.', 'year': '2020', 'runtime': '107 min', 'rating': '7.1', 'votes': '121,353', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt1489887', 'title': 'Booksmart', 'rank': '130.', 'year': '2019', 'runtime': '102 min', 'rating': '7.1', 'votes': '120,057', 'genres': 'Comedy            '}, {'movie_id': 'tt2557478', 'title': 'Pacific Rim: Uprising', 'rank': '131.', 'year': '2018', 'runtime': '111 min', 'rating': '5.6', 'votes': '118,362', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt7737786', 'title': 'Greenland', 'rank': '132.', 'year': '2020', 'runtime': '119 min', 'rating': '6.4', 'votes': '117,784', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt1590193', 'title': 'The Commuter', 'rank': '133.', 'year': '2018', 'runtime': '104 min', 'rating': '6.3', 'votes': '116,801', 'genres': 'Action, Mystery, Thriller            '}, {'movie_id': 'tt10189514', 'title': 'Soorarai Pottru', 'rank': '134.', 'year': '2020', 'runtime': '153 min', 'rating': '8.7', 'votes': '116,747', 'genres': 'Drama            '}, {'movie_id': 'tt5164432', 'title': 'Love, Simon', 'rank': '135.', 'year': '2018', 'runtime': '110 min', 'rating': '7.5', 'votes': '115,880', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt6394270', 'title': 'Bombshell', 'rank': '136.', 'year': '2019', 'runtime': '109 min', 'rating': '6.8', 'votes': '115,850', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt8244784', 'title': 'The Hunt', 'rank': '137.', 'year': '2020', 'runtime': '90 min', 'rating': '6.5', 'votes': '115,334', 'genres': 'Action, Horror, Thriller            '}, {'movie_id': 'tt1025100', 'title': 'Gemini Man', 'rank': '138.', 'year': '2019', 'runtime': '117 min', 'rating': '5.7', 'votes': '114,286', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt1259528', 'title': 'Den of Thieves', 'rank': '139.', 'year': '2018', 'runtime': '140 min', 'rating': '7.0', 'votes': '113,067', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt2139881', 'title': 'Long Shot', 'rank': '140.', 'year': '2019', 'runtime': '125 min', 'rating': '6.8', 'votes': '112,167', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt3846674', 'title': "To All the Boys I've Loved Before", 'rank': '141.', 'year': '2018', 'runtime': '99 min', 'rating': '7.0', 'votes': '108,777', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt7401588', 'title': 'Instant Family', 'rank': '142.', 'year': '2018', 'runtime': '118 min', 'rating': '7.3', 'votes': '108,012', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt4777008', 'title': 'Maleficent: Mistress of Evil', 'rank': '143.', 'year': '2019', 'runtime': '119 min', 'rating': '6.6', 'votes': '106,127', 'genres': 'Adventure, Family, Fantasy            '}, {'movie_id': 'tt2548396', 'title': 'The Cloverfield Paradox', 'rank': '144.', 'year': '2018', 'runtime': '102 min', 'rating': '5.5', 'votes': '106,127', 'genres': 'Action, Horror, Sci-Fi            '}, {'movie_id': 'tt5503686', 'title': 'Hustlers', 'rank': '145.', 'year': '2019', 'runtime': '110 min', 'rating': '6.3', 'votes': '100,913', 'genres': 'Comedy, Crime, Drama            '}, {'movie_id': 'tt4530422', 'title': 'Overlord', 'rank': '146.', 'year': '2018', 'runtime': '110 min', 'rating': '6.6', 'votes': '100,051', 'genres': 'Action, Horror, Sci-Fi            '}, {'movie_id': 'tt1206885', 'title': 'Rambo: Last Blood', 'rank': '147.', 'year': '2019', 'runtime': '89 min', 'rating': '6.1', 'votes': '99,338', 'genres': 'Action, Crime, Thriller            '}, {'movie_id': 'tt7752126', 'title': 'Brightburn', 'rank': '148.', 'year': '2019', 'runtime': '90 min', 'rating': '6.1', 'votes': '99,009', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt6189022', 'title': 'Angel Has Fallen', 'rank': '149.', 'year': '2019', 'runtime': '121 min', 'rating': '6.4', 'votes': '98,617', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt4218572', 'title': 'Widows', 'rank': '150.', 'year': '2018', 'runtime': '129 min', 'rating': '6.8', 'votes': '98,427', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt6048922', 'title': 'Greyhound', 'rank': '151.', 'year': '2020', 'runtime': '91 min', 'rating': '7.0', 'votes': '97,966', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt6911608', 'title': 'Mamma Mia! Here We Go Again', 'rank': '152.', 'year': '2018', 'runtime': '114 min', 'rating': '6.6', 'votes': '96,271', 'genres': 'Comedy, Musical, Romance            '}, {'movie_id': 'tt7886848', 'title': 'Sadak 2', 'rank': '153.', 'year': '2020', 'runtime': '133 min', 'rating': '1.1', 'votes': '95,972', 'genres': 'Action, Drama            '}, {'movie_id': 'tt2481498', 'title': 'Extremely Wicked, Shockingly Evil and Vile', 'rank': '154.', 'year': '2019', 'runtime': '110 min', 'rating': '6.7', 'votes': '95,250', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt8580274', 'title': 'Eurovision Song Contest: The Story of Fire Saga', 'rank': '155.', 'year': '2020', 'runtime': '123 min', 'rating': '6.5', 'votes': '94,634', 'genres': 'Comedy, Musical            '}, {'movie_id': 'tt8503618', 'title': 'Hamilton', 'rank': '156.', 'year': '2020', 'runtime': '160 min', 'rating': '8.4', 'votes': '94,561', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt8108198', 'title': 'Andhadhun', 'rank': '157.', 'year': '2018', 'runtime': '139 min', 'rating': '8.2', 'votes': '93,926', 'genres': 'Comedy, Crime, Music            '}, {'movie_id': 'tt0837563', 'title': 'Pet Sematary', 'rank': '158.', 'year': '2019', 'runtime': '101 min', 'rating': '5.7', 'votes': '92,967', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt1860242', 'title': 'The Highwaymen', 'rank': '159.', 'year': '2019', 'runtime': '132 min', 'rating': '6.9', 'votes': '92,254', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt8613070', 'title': 'Portrait of a Lady on Fire', 'rank': '160.', 'year': '2019', 'runtime': '122 min', 'rating': '8.1', 'votes': '92,088', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt6292852', 'title': 'I Am Mother', 'rank': '161.', 'year': '2019', 'runtime': '113 min', 'rating': '6.7', 'votes': '91,855', 'genres': 'Drama, Mystery, Sci-Fi            '}, {'movie_id': 'tt2274648', 'title': 'Hellboy', 'rank': '162.', 'year': '2019', 'runtime': '120 min', 'rating': '5.2', 'votes': '91,189', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt4364194', 'title': 'The Peanut Butter Falcon', 'rank': '163.', 'year': '2019', 'runtime': '97 min', 'rating': '7.6', 'votes': '90,968', 'genres': 'Adventure, Comedy, Drama            '}, {'movie_id': 'tt4139588', 'title': 'Polar', 'rank': '164.', 'year': '2019', 'runtime': '118 min', 'rating': '6.3', 'votes': '90,874', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt10886166', 'title': '365 Days', 'rank': '165.', 'year': '2020', 'runtime': '114 min', 'rating': '3.3', 'votes': '90,341', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt8267604', 'title': 'Capernaum', 'rank': '166.', 'year': '2018', 'runtime': '126 min', 'rating': '8.4', 'votes': '89,953', 'genres': 'Drama            '}, {'movie_id': 'tt9071322', 'title': 'Dark Waters', 'rank': '167.', 'year': '2019', 'runtime': '126 min', 'rating': '7.6', 'votes': '89,374', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt7550000', 'title': 'Project Power', 'rank': '168.', 'year': '2020', 'runtime': '113 min', 'rating': '6.0', 'votes': '89,277', 'genres': 'Action, Crime, Sci-Fi            '}, {'movie_id': 'tt8629748', 'title': 'Spenser Confidential', 'rank': '169.', 'year': '2020', 'runtime': '111 min', 'rating': '6.2', 'votes': '89,116', 'genres': 'Action, Comedy, Drama            '}, {'movie_id': 'tt5028340', 'title': 'Mary Poppins Returns', 'rank': '170.', 'year': '2018', 'runtime': '130 min', 'rating': '6.7', 'votes': '88,841', 'genres': 'Adventure, Comedy, Family            '}, {'movie_id': 'tt3513548', 'title': 'Richard Jewell', 'rank': '171.', 'year': '2019', 'runtime': '131 min', 'rating': '7.5', 'votes': '88,079', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt6878306', 'title': 'News of the World', 'rank': '172.', 'year': '2020', 'runtime': '118 min', 'rating': '6.8', 'votes': '87,541', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt7939766', 'title': "I'm Thinking of Ending Things", 'rank': '173.', 'year': '2020', 'runtime': '134 min', 'rating': '6.6', 'votes': '87,260', 'genres': 'Drama, Thriller            '}, {'movie_id': 'tt7838252', 'title': 'K.G.F: Chapter 1', 'rank': '174.', 'year': '2018', 'runtime': '156 min', 'rating': '8.2', 'votes': '86,915', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt6924650', 'title': 'Midway', 'rank': '175.', 'year': '2019', 'runtime': '138 min', 'rating': '6.7', 'votes': '86,345', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt8364368', 'title': 'Crawl', 'rank': '176.', 'year': '2019', 'runtime': '87 min', 'rating': '6.1', 'votes': '85,343', 'genres': 'Action, Adventure, Horror            '}, {'movie_id': 'tt3799232', 'title': 'The Kissing Booth', 'rank': '177.', 'year': '2018', 'runtime': '105 min', 'rating': '5.9', 'votes': '84,974', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt10539608', 'title': 'The Midnight Sky', 'rank': '178.', 'year': '2020', 'runtime': '118 min', 'rating': '5.6', 'votes': '83,774', 'genres': 'Adventure, Drama, Sci-Fi            '}, {'movie_id': 'tt5774060', 'title': 'Underwater', 'rank': '179.', 'year': '2020', 'runtime': '95 min', 'rating': '5.8', 'votes': '83,624', 'genres': 'Action, Horror, Sci-Fi            '}, {'movie_id': 'tt7456310', 'title': 'Anna', 'rank': '180.', 'year': '2019', 'runtime': '118 min', 'rating': '6.6', 'votes': '82,956', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt1413492', 'title': '12 Strong', 'rank': '181.', 'year': '2018', 'runtime': '130 min', 'rating': '6.5', 'votes': '82,826', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt6513120', 'title': 'Fighting with My Family', 'rank': '182.', 'year': '2019', 'runtime': '108 min', 'rating': '7.1', 'votes': '81,991', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt10633456', 'title': 'Minari', 'rank': '183.', 'year': '2020', 'runtime': '115 min', 'rating': '7.4', 'votes': '81,799', 'genres': 'Drama            '}, {'movie_id': 'tt10554232', 'title': 'Dara of Jasenovac', 'rank': '184.', 'year': '2020', 'runtime': '130 min', 'rating': '8.2', 'votes': '81,058', 'genres': 'Drama, War            '}, {'movie_id': 'tt3224458', 'title': 'A Beautiful Day in the Neighborhood', 'rank': '185.', 'year': '2019', 'runtime': '109 min', 'rating': '7.2', 'votes': '80,712', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt4560436', 'title': 'Mile 22', 'rank': '186.', 'year': '2018', 'runtime': '94 min', 'rating': '6.1', 'votes': '80,548', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt5688932', 'title': 'Sorry to Bother You', 'rank': '187.', 'year': '2018', 'runtime': '112 min', 'rating': '6.9', 'votes': '80,419', 'genres': 'Comedy, Drama, Fantasy            '}, {'movie_id': 'tt4682266', 'title': 'The New Mutants', 'rank': '188.', 'year': '2020', 'runtime': '94 min', 'rating': '5.3', 'votes': '80,325', 'genres': 'Action, Horror, Mystery            '}, {'movie_id': 'tt2531344', 'title': 'Blockers', 'rank': '189.', 'year': '2018', 'runtime': '102 min', 'rating': '6.2', 'votes': '80,292', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt6998518', 'title': 'Mandy', 'rank': '190.', 'year': '2018', 'runtime': '121 min', 'rating': '6.5', 'votes': '80,264', 'genres': 'Action, Fantasy, Horror            '}, {'movie_id': 'tt8633478', 'title': 'Run', 'rank': '191.', 'year': '2020', 'runtime': '90 min', 'rating': '6.7', 'votes': '80,236', 'genres': 'Mystery, Thriller            '}, {'movie_id': 'tt1034415', 'title': 'Suspiria', 'rank': '192.', 'year': '2018', 'runtime': '152 min', 'rating': '6.7', 'votes': '79,504', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt8695030', 'title': "The Dead Don't Die", 'rank': '193.', 'year': '2019', 'runtime': '104 min', 'rating': '5.4', 'votes': '79,272', 'genres': 'Comedy, Fantasy, Horror            '}, {'movie_id': 'tt8623904', 'title': 'Last Christmas', 'rank': '194.', 'year': '2019', 'runtime': '103 min', 'rating': '6.5', 'votes': '78,718', 'genres': 'Comedy, Drama, Fantasy            '}, {'movie_id': 'tt4575576', 'title': 'Christopher Robin', 'rank': '195.', 'year': '2018', 'runtime': '104 min', 'rating': '7.2', 'votes': '78,659', 'genres': 'Adventure, Comedy, Drama            '}, {'movie_id': 'tt1634106', 'title': 'Bloodshot', 'rank': '196.', 'year': '2020', 'runtime': '109 min', 'rating': '5.7', 'votes': '78,504', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt6663582', 'title': 'The Spy Who Dumped Me', 'rank': '197.', 'year': '2018', 'runtime': '117 min', 'rating': '6.0', 'votes': '78,369', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt1226837', 'title': 'Beautiful Boy', 'rank': '198.', 'year': '2018', 'runtime': '120 min', 'rating': '7.3', 'votes': '78,369', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt3387520', 'title': 'Scary Stories to Tell in the Dark', 'rank': '199.', 'year': '2019', 'runtime': '108 min', 'rating': '6.2', 'votes': '77,754', 'genres': 'Adventure, Horror, Mystery            '}, {'movie_id': 'tt4003440', 'title': 'The House That Jack Built', 'rank': '200.', 'year': '2018', 'runtime': '152 min', 'rating': '6.8', 'votes': '77,743', 'genres': 'Crime, Drama, Horror            '}, {'movie_id': 'tt8075192', 'title': 'Shoplifters', 'rank': '201.', 'year': '2018', 'runtime': '121 min', 'rating': '7.9', 'votes': '77,220', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt8350360', 'title': 'Annabelle Comes Home', 'rank': '202.', 'year': '2019', 'runtime': '106 min', 'rating': '5.9', 'votes': '76,679', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt3861390', 'title': 'Dumbo', 'rank': '203.', 'year': '2019', 'runtime': '112 min', 'rating': '6.3', 'votes': '76,633', 'genres': 'Adventure, Family, Fantasy            '}, {'movie_id': 'tt7014006', 'title': 'Eighth Grade', 'rank': '204.', 'year': '2018', 'runtime': '93 min', 'rating': '7.4', 'votes': '76,456', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt4332232', 'title': 'Fractured', 'rank': '205.', 'year': '2019', 'runtime': '99 min', 'rating': '6.4', 'votes': '76,347', 'genres': 'Thriller            '}, {'movie_id': 'tt10618286', 'title': 'Mank', 'rank': '206.', 'year': '2020', 'runtime': '131 min', 'rating': '6.8', 'votes': '75,796', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt8155288', 'title': 'Happy Death Day 2U', 'rank': '207.', 'year': '2019', 'runtime': '100 min', 'rating': '6.2', 'votes': '75,531', 'genres': 'Comedy, Horror, Mystery            '}, {'movie_id': 'tt5220122', 'title': 'Hotel Transylvania 3: Summer Vacation', 'rank': '208.', 'year': '2018', 'runtime': '97 min', 'rating': '6.3', 'votes': '75,163', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt6921996', 'title': 'Johnny English Strikes Again', 'rank': '209.', 'year': '2018', 'runtime': '89 min', 'rating': '6.2', 'votes': '75,119', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt6977338', 'title': 'Good Boys', 'rank': '210.', 'year': '2019', 'runtime': '90 min', 'rating': '6.7', 'votes': '74,922', 'genres': 'Adventure, Comedy            '}, {'movie_id': 'tt4532826', 'title': 'Robin Hood', 'rank': '211.', 'year': '2018', 'runtime': '116 min', 'rating': '5.3', 'votes': '74,567', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt5033998', 'title': "Charlie's Angels", 'rank': '212.', 'year': '2019', 'runtime': '118 min', 'rating': '4.9', 'votes': '74,246', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt10280276', 'title': 'Coolie No. 1', 'rank': '213.', 'year': '2020', 'runtime': '134 min', 'rating': '4.2', 'votes': '73,650', 'genres': 'Action, Comedy, Romance            '}, {'movie_id': 'tt2990140', 'title': 'The Christmas Chronicles', 'rank': '214.', 'year': '2018', 'runtime': '104 min', 'rating': '7.0', 'votes': '73,326', 'genres': 'Adventure, Comedy, Family            '}, {'movie_id': 'tt6679794', 'title': 'Outlaw King', 'rank': '215.', 'year': '2018', 'runtime': '121 min', 'rating': '6.9', 'votes': '73,019', 'genres': 'Action, Biography, Drama            '}, {'movie_id': 'tt1137450', 'title': 'Death Wish', 'rank': '216.', 'year': '2018', 'runtime': '107 min', 'rating': '6.3', 'votes': '72,793', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt2709692', 'title': 'The Grinch', 'rank': '217.', 'year': '2018', 'runtime': '85 min', 'rating': '6.3', 'votes': '71,456', 'genres': 'Animation, Comedy, Family            '}, {'movie_id': 'tt2452244', 'title': "Isn't It Romantic", 'rank': '218.', 'year': '2019', 'runtime': '89 min', 'rating': '5.9', 'votes': '70,531', 'genres': 'Comedy, Fantasy, Musical            '}, {'movie_id': 'tt5719748', 'title': 'Cold Pursuit', 'rank': '219.', 'year': '2019', 'runtime': '119 min', 'rating': '6.2', 'votes': '69,958', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt8359848', 'title': 'Climax', 'rank': '220.', 'year': '2018', 'runtime': '97 min', 'rating': '6.9', 'votes': '69,470', 'genres': 'Drama, Horror, Music            '}, {'movie_id': 'tt10059518', 'title': 'Unhinged', 'rank': '221.', 'year': '2020', 'runtime': '90 min', 'rating': '6.0', 'votes': '69,304', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt9686708', 'title': 'The King of Staten Island', 'rank': '222.', 'year': '2020', 'runtime': '136 min', 'rating': '7.1', 'votes': '68,697', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt4916630', 'title': 'Just Mercy', 'rank': '223.', 'year': '2019', 'runtime': '137 min', 'rating': '7.6', 'votes': '68,287', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt7638348', 'title': 'Boss Level', 'rank': '224.', 'year': '2020', 'runtime': '100 min', 'rating': '6.8', 'votes': '68,283', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt8688634', 'title': '21 Bridges', 'rank': '225.', 'year': '2019', 'runtime': '99 min', 'rating': '6.6', 'votes': '68,062', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt6472976', 'title': 'Five Feet Apart', 'rank': '226.', 'year': '2019', 'runtime': '116 min', 'rating': '7.2', 'votes': '67,916', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt3513498', 'title': 'The Lego Movie 2: The Second Part', 'rank': '227.', 'year': '2019', 'runtime': '107 min', 'rating': '6.6', 'votes': '67,523', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt4477536', 'title': 'Fifty Shades Freed', 'rank': '228.', 'year': '2018', 'runtime': '105 min', 'rating': '4.5', 'votes': '66,784', 'genres': 'Drama, Romance, Thriller            '}, {'movie_id': 'tt9866072', 'title': 'Holidate', 'rank': '229.', 'year': '2020', 'runtime': '104 min', 'rating': '6.1', 'votes': '66,699', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt5613484', 'title': 'Mid90s', 'rank': '230.', 'year': '2018', 'runtime': '85 min', 'rating': '7.3', 'votes': '66,371', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt7282468', 'title': 'Burning', 'rank': '231.', 'year': '2018', 'runtime': '148 min', 'rating': '7.5', 'votes': '65,679', 'genres': 'Drama, Mystery, Thriller            '}, {'movie_id': 'tt6850820', 'title': 'Peppermint', 'rank': '232.', 'year': '2018', 'runtime': '101 min', 'rating': '6.5', 'votes': '65,629', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt4971344', 'title': 'The Sisters Brothers', 'rank': '233.', 'year': '2018', 'runtime': '122 min', 'rating': '6.9', 'votes': '65,472', 'genres': 'Drama, Western            '}, {'movie_id': 'tt6673612', 'title': 'Dolittle', 'rank': '234.', 'year': '2020', 'runtime': '101 min', 'rating': '5.6', 'votes': '65,172', 'genres': 'Adventure, Comedy, Family            '}, {'movie_id': 'tt8637428', 'title': 'The Farewell', 'rank': '235.', 'year': '2019', 'runtime': '100 min', 'rating': '7.5', 'votes': '64,930', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt5461944', 'title': 'Hotel Mumbai', 'rank': '236.', 'year': '2018', 'runtime': '123 min', 'rating': '7.6', 'votes': '64,813', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt6133466', 'title': 'The First Purge', 'rank': '237.', 'year': '2018', 'runtime': '97 min', 'rating': '5.2', 'votes': '64,779', 'genres': 'Action, Horror, Sci-Fi            '}, {'movie_id': 'tt8291224', 'title': 'Uri: The Surgical Strike', 'rank': '238.', 'year': '2019', 'runtime': '138 min', 'rating': '8.2', 'votes': '64,482', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt1846589', 'title': 'Hunter Killer', 'rank': '239.', 'year': '2018', 'runtime': '121 min', 'rating': '6.6', 'votes': '64,208', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt4244998', 'title': 'Alpha', 'rank': '240.', 'year': '2018', 'runtime': '96 min', 'rating': '6.6', 'votes': '63,094', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt5113040', 'title': 'The Secret Life of Pets 2', 'rank': '241.', 'year': '2019', 'runtime': '86 min', 'rating': '6.4', 'votes': '62,876', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt2388771', 'title': 'Mowgli: Legend of the Jungle', 'rank': '242.', 'year': '2018', 'runtime': '104 min', 'rating': '6.5', 'votes': '62,531', 'genres': 'Adventure, Drama, Fantasy            '}, {'movie_id': 'tt6902676', 'title': 'Guns Akimbo', 'rank': '243.', 'year': '2019', 'runtime': '98 min', 'rating': '6.3', 'votes': '62,427', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt7043012', 'title': 'Velvet Buzzsaw', 'rank': '244.', 'year': '2019', 'runtime': '113 min', 'rating': '5.7', 'votes': '61,942', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt8368512', 'title': 'The Courier', 'rank': '245.', 'year': '2020', 'runtime': '112 min', 'rating': '7.2', 'votes': '61,842', 'genres': 'Drama, History, Thriller            '}, {'movie_id': 'tt5726086', 'title': 'Insidious: The Last Key', 'rank': '246.', 'year': '2018', 'runtime': '103 min', 'rating': '5.7', 'votes': '61,502', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt8526872', 'title': 'Dolemite Is My Name', 'rank': '247.', 'year': '2019', 'runtime': '118 min', 'rating': '7.2', 'votes': '61,326', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt1298644', 'title': 'The Hustle', 'rank': '248.', 'year': '2019', 'runtime': '93 min', 'rating': '5.4', 'votes': '60,425', 'genres': 'Comedy, Crime            '}, {'movie_id': 'tt3892172', 'title': 'Leave No Trace', 'rank': '249.', 'year': '2018', 'runtime': '109 min', 'rating': '7.1', 'votes': '60,366', 'genres': 'Adventure, Drama            '}, {'movie_id': 'tt8368406', 'title': 'Vivarium', 'rank': '250.', 'year': '2019', 'runtime': '97 min', 'rating': '5.8', 'votes': '60,196', 'genres': 'Horror, Mystery, Sci-Fi            '}, {'movie_id': 'tt5610554', 'title': 'Tully', 'rank': '251.', 'year': '2018', 'runtime': '95 min', 'rating': '6.9', 'votes': '59,824', 'genres': 'Comedy, Drama, Mystery            '}, {'movie_id': 'tt6475714', 'title': 'Monster Hunter', 'rank': '252.', 'year': '2020', 'runtime': '103 min', 'rating': '5.2', 'votes': '59,654', 'genres': 'Action, Adventure, Fantasy            '}, {'movie_id': 'tt6742252', 'title': 'The Guilty', 'rank': '253.', 'year': '2018', 'runtime': '85 min', 'rating': '7.5', 'votes': '59,534', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt10919380', 'title': 'Freaky', 'rank': '254.', 'year': '2020', 'runtime': '102 min', 'rating': '6.3', 'votes': '59,013', 'genres': 'Comedy, Horror, Thriller            '}, {'movie_id': 'tt8291806', 'title': 'Pain and Glory', 'rank': '255.', 'year': '2019', 'runtime': '113 min', 'rating': '7.5', 'votes': '58,732', 'genres': 'Drama            '}, {'movie_id': 'tt11032374', 'title': 'Demon Slayer the Movie: Mugen Train', 'rank': '256.', 'year': '2020', 'runtime': '117 min', 'rating': '8.2', 'votes': '58,154', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt0385887', 'title': 'Motherless Brooklyn', 'rank': '257.', 'year': '2019', 'runtime': '144 min', 'rating': '6.8', 'votes': '58,022', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt6543652', 'title': 'Cold War', 'rank': '258.', 'year': '2018', 'runtime': '89 min', 'rating': '7.5', 'votes': '57,613', 'genres': 'Drama, Music, Romance            '}, {'movie_id': 'tt10350922', 'title': 'Laxmii', 'rank': '259.', 'year': '2020', 'runtime': '141 min', 'rating': '2.6', 'votes': '57,508', 'genres': 'Action, Comedy, Horror            '}, {'movie_id': 'tt6306064', 'title': 'Adrift', 'rank': '260.', 'year': '2018', 'runtime': '96 min', 'rating': '6.6', 'votes': '57,445', 'genres': 'Action, Adventure, Biography            '}, {'movie_id': 'tt4687108', 'title': 'In the Tall Grass', 'rank': '261.', 'year': '2019', 'runtime': '101 min', 'rating': '5.4', 'votes': '57,272', 'genres': 'Horror, Mystery, Sci-Fi            '}, {'movie_id': 'tt6772950', 'title': 'Truth or Dare', 'rank': '262.', 'year': '2018', 'runtime': '100 min', 'rating': '5.2', 'votes': '57,124', 'genres': 'Horror, Thriller            '}, {'movie_id': 'tt7374948', 'title': 'Always Be My Maybe', 'rank': '263.', 'year': '2019', 'runtime': '101 min', 'rating': '6.8', 'votes': '57,079', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt6398184', 'title': 'Downton Abbey', 'rank': '264.', 'year': '2019', 'runtime': '122 min', 'rating': '7.4', 'votes': '56,946', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt6908274', 'title': 'Mirage', 'rank': '265.', 'year': '2018', 'runtime': '128 min', 'rating': '7.4', 'votes': '56,709', 'genres': 'Drama, Fantasy, Mystery            '}, {'movie_id': 'tt4126476', 'title': 'After', 'rank': '266.', 'year': '2019', 'runtime': '105 min', 'rating': '5.3', 'votes': '56,295', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt8784956', 'title': 'Ava', 'rank': '267.', 'year': 'V2020', 'runtime': '96 min', 'rating': '5.4', 'votes': '56,228', 'genres': 'Action, Drama, Thriller            '}, {'movie_id': 'tt6452574', 'title': 'Sanju', 'rank': '268.', 'year': '2018', 'runtime': '155 min', 'rating': '7.6', 'votes': '55,342', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt7668842', 'title': 'Enes Batur', 'rank': '269.', 'year': '2018', 'runtime': '110 min', 'rating': '2.0', 'votes': '55,290', 'genres': 'Comedy            '}, {'movie_id': 'tt7825208', 'title': 'Marighella', 'rank': '270.', 'year': '2019', 'runtime': '155 min', 'rating': '6.6', 'votes': '55,248', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt9052870', 'title': 'Chhichhore', 'rank': '271.', 'year': '2019', 'runtime': '143 min', 'rating': '8.3', 'votes': '54,935', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt5304992', 'title': 'Set It Up', 'rank': '272.', 'year': '2018', 'runtime': '105 min', 'rating': '6.5', 'votes': '54,919', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt9214832', 'title': 'Emma.', 'rank': '273.', 'year': '2020', 'runtime': '124 min', 'rating': '6.7', 'votes': '54,890', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt5968394', 'title': 'Captive State', 'rank': '274.', 'year': '2019', 'runtime': '109 min', 'rating': '6.0', 'votes': '54,834', 'genres': 'Action, Horror, Sci-Fi            '}, {'movie_id': 'tt6791096', 'title': 'I Feel Pretty', 'rank': '275.', 'year': '2018', 'runtime': '110 min', 'rating': '5.6', 'votes': '54,418', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt5834262', 'title': 'Hotel Artemis', 'rank': '276.', 'year': '2018', 'runtime': '94 min', 'rating': '6.1', 'votes': '54,226', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt4463894', 'title': 'Shaft', 'rank': '277.', 'year': '2019', 'runtime': '111 min', 'rating': '6.4', 'votes': '54,188', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt7958736', 'title': 'Ma', 'rank': '278.', 'year': '2019', 'runtime': '99 min', 'rating': '5.6', 'votes': '54,006', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt9731534', 'title': 'The Night House', 'rank': '279.', 'year': '2020', 'runtime': '107 min', 'rating': '6.5', 'votes': '53,961', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt8110640', 'title': 'In the Shadow of the Moon', 'rank': '280.', 'year': '2019', 'runtime': '115 min', 'rating': '6.2', 'votes': '53,927', 'genres': 'Action, Crime, Mystery            '}, {'movie_id': 'tt10514222', 'title': "Ma Rainey's Black Bottom", 'rank': '281.', 'year': '2020', 'runtime': '94 min', 'rating': '6.9', 'votes': '53,918', 'genres': 'Drama, Music            '}, {'movie_id': 'tt5246700', 'title': 'How It Ends', 'rank': '282.', 'year': '2018', 'runtime': '113 min', 'rating': '5.0', 'votes': '53,821', 'genres': 'Action, Drama, Horror            '}, {'movie_id': 'tt6217306', 'title': 'Apostle', 'rank': '283.', 'year': '2018', 'runtime': '130 min', 'rating': '6.3', 'votes': '53,813', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt3201640', 'title': 'Extinction', 'rank': '284.', 'year': '2018', 'runtime': '95 min', 'rating': '5.8', 'votes': '53,564', 'genres': 'Action, Drama, Sci-Fi            '}, {'movie_id': 'tt0983946', 'title': 'Fantasy Island', 'rank': '285.', 'year': '2020', 'runtime': '109 min', 'rating': '4.9', 'votes': '53,057', 'genres': 'Fantasy, Horror, Mystery            '}, {'movie_id': 'tt4595882', 'title': 'Can You Ever Forgive Me?', 'rank': '286.', 'year': '2018', 'runtime': '106 min', 'rating': '7.1', 'votes': '52,828', 'genres': 'Biography, Comedy, Crime            '}, {'movie_id': 'tt6820256', 'title': 'Arctic', 'rank': '287.', 'year': '2018', 'runtime': '98 min', 'rating': '6.8', 'votes': '52,823', 'genres': 'Adventure, Drama            '}, {'movie_id': 'tt8663516', 'title': "Child's Play", 'rank': '288.', 'year': '2019', 'runtime': '90 min', 'rating': '5.7', 'votes': '52,653', 'genres': 'Drama, Horror, Sci-Fi            '}, {'movie_id': 'tt2119543', 'title': 'The House with a Clock in Its Walls', 'rank': '289.', 'year': '2018', 'runtime': '105 min', 'rating': '6.0', 'votes': '52,331', 'genres': 'Comedy, Family, Fantasy            '}, {'movie_id': 'tt9777644', 'title': 'Da 5 Bloods', 'rank': '290.', 'year': '2020', 'runtime': '154 min', 'rating': '6.5', 'votes': '51,906', 'genres': 'Adventure, Drama, War            '}, {'movie_id': 'tt5697572', 'title': 'Cats', 'rank': '291.', 'year': '2019', 'runtime': '110 min', 'rating': '2.8', 'votes': '51,739', 'genres': 'Comedy, Drama, Family            '}, {'movie_id': 'tt10682266', 'title': 'Hubie Halloween', 'rank': '292.', 'year': '2020', 'runtime': '103 min', 'rating': '5.2', 'votes': '51,649', 'genres': 'Comedy, Mystery            '}, {'movie_id': 'tt5865326', 'title': 'The Laundromat', 'rank': '293.', 'year': '2019', 'runtime': '96 min', 'rating': '6.3', 'votes': '51,433', 'genres': 'Comedy, Crime, Drama            '}, {'movie_id': 'tt2328900', 'title': 'Mary Queen of Scots', 'rank': '294.', 'year': '2018', 'runtime': '124 min', 'rating': '6.3', 'votes': '51,188', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt11655202', 'title': 'Riders of Justice', 'rank': '295.', 'year': '2020', 'runtime': '116 min', 'rating': '7.5', 'votes': '51,132', 'genres': 'Action, Comedy, Drama            '}, {'movie_id': 'tt4913966', 'title': 'The Curse of La Llorona', 'rank': '296.', 'year': '2019', 'runtime': '93 min', 'rating': '5.2', 'votes': '51,113', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt7549996', 'title': 'Judy', 'rank': '297.', 'year': '2019', 'runtime': '118 min', 'rating': '6.8', 'votes': '51,097', 'genres': 'Biography, Drama, Romance            '}, {'movie_id': 'tt1838556', 'title': 'Honest Thief', 'rank': '298.', 'year': '2020', 'runtime': '99 min', 'rating': '6.0', 'votes': '50,985', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt11161474', 'title': 'Pieces of a Woman', 'rank': '299.', 'year': '2020', 'runtime': '126 min', 'rating': '7.0', 'votes': '50,395', 'genres': 'Drama            '}, {'movie_id': 'tt6079516', 'title': 'I See You', 'rank': '300.', 'year': '2019', 'runtime': '98 min', 'rating': '6.8', 'votes': '50,330', 'genres': 'Crime, Drama, Horror            '}, {'movie_id': 'tt0800325', 'title': 'The Dirt', 'rank': '301.', 'year': '2019', 'runtime': '107 min', 'rating': '7.0', 'votes': '50,318', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt5783956', 'title': 'When We First Met', 'rank': '302.', 'year': '2018', 'runtime': '97 min', 'rating': '6.4', 'votes': '49,980', 'genres': 'Comedy, Fantasy, Romance            '}, {'movie_id': 'tt5814534', 'title': 'Spies in Disguise', 'rank': '303.', 'year': '2019', 'runtime': '102 min', 'rating': '6.8', 'votes': '49,893', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt7504726', 'title': 'The Call of the Wild', 'rank': '304.', 'year': '2020', 'runtime': '100 min', 'rating': '6.7', 'votes': '49,773', 'genres': 'Adventure, Drama, Family            '}, {'movie_id': 'tt5116302', 'title': 'Togo', 'rank': '305.', 'year': '2019', 'runtime': '113 min', 'rating': '7.9', 'votes': '49,470', 'genres': 'Adventure, Biography, Drama            '}, {'movie_id': 'tt8239946', 'title': 'Tumbbad', 'rank': '306.', 'year': '2018', 'runtime': '104 min', 'rating': '8.2', 'votes': '49,389', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt7125860', 'title': 'If Beale Street Could Talk', 'rank': '307.', 'year': '2018', 'runtime': '119 min', 'rating': '7.1', 'votes': '49,341', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt10431500', 'title': 'Miracle in Cell No. 7', 'rank': '308.', 'year': '2019', 'runtime': '132 min', 'rating': '8.2', 'votes': '49,299', 'genres': 'Drama            '}, {'movie_id': 'tt5073642', 'title': 'Color Out of Space', 'rank': '309.', 'year': '2019', 'runtime': '111 min', 'rating': '6.2', 'votes': '48,856', 'genres': 'Horror, Mystery, Sci-Fi            '}, {'movie_id': 'tt7347846', 'title': 'The Lodge', 'rank': '310.', 'year': '2019', 'runtime': '108 min', 'rating': '6.0', 'votes': '48,586', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt5431890', 'title': 'Official Secrets', 'rank': '311.', 'year': '2019', 'runtime': '112 min', 'rating': '7.3', 'votes': '48,425', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt5177088', 'title': "The Girl in the Spider's Web", 'rank': '312.', 'year': '2018', 'runtime': '115 min', 'rating': '6.1', 'votes': '48,259', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt1086064', 'title': 'Bill & Ted Face the Music', 'rank': '313.', 'year': '2020', 'runtime': '91 min', 'rating': '5.9', 'votes': '48,250', 'genres': 'Adventure, Comedy, Music            '}, {'movie_id': 'tt7734218', 'title': 'Stuber', 'rank': '314.', 'year': '2019', 'runtime': '93 min', 'rating': '6.2', 'votes': '48,032', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt1289403', 'title': 'The Guernsey Literary and Potato Peel Pie Society', 'rank': '315.', 'year': '2018', 'runtime': '124 min', 'rating': '7.3', 'votes': '47,346', 'genres': 'Drama, Romance, War            '}, {'movie_id': 'tt6491178', 'title': 'Dragged Across Concrete', 'rank': '316.', 'year': '2018', 'runtime': '159 min', 'rating': '6.9', 'votes': '47,094', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt8544498', 'title': 'The Way Back', 'rank': '317.', 'year': '2020', 'runtime': '108 min', 'rating': '6.7', 'votes': '47,008', 'genres': 'Drama, Sport            '}, {'movie_id': 'tt7153766', 'title': 'Unsane', 'rank': '318.', 'year': '2018', 'runtime': '98 min', 'rating': '6.4', 'votes': '46,973', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt7431594', 'title': 'Race 3', 'rank': '319.', 'year': '2018', 'runtime': '160 min', 'rating': '1.9', 'votes': '46,888', 'genres': 'Action, Crime, Thriller            '}, {'movie_id': 'tt2837574', 'title': 'The Old Man & the Gun', 'rank': '320.', 'year': '2018', 'runtime': '93 min', 'rating': '6.7', 'votes': '46,645', 'genres': 'Biography, Comedy, Crime            '}, {'movie_id': 'tt5932728', 'title': 'The Professor and the Madman', 'rank': '321.', 'year': '2019', 'runtime': '124 min', 'rating': '7.2', 'votes': '46,563', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt7315484', 'title': 'The Silence', 'rank': '322.', 'year': '2019', 'runtime': '90 min', 'rating': '5.3', 'votes': '45,999', 'genres': 'Drama, Horror, Sci-Fi            '}, {'movie_id': 'tt6212478', 'title': 'American Animals', 'rank': '323.', 'year': '2018', 'runtime': '116 min', 'rating': '7.0', 'votes': '45,508', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt1620680', 'title': 'A Wrinkle in Time', 'rank': '324.', 'year': '2018', 'runtime': '109 min', 'rating': '4.2', 'votes': '45,394', 'genres': 'Adventure, Family, Fantasy            '}, {'movie_id': 'tt5397194', 'title': 'Anon', 'rank': '325.', 'year': '2018', 'runtime': '100 min', 'rating': '6.1', 'votes': '45,109', 'genres': 'Crime, Mystery, Sci-Fi            '}, {'movie_id': 'tt7060344', 'title': 'Raatchasan', 'rank': '326.', 'year': '2018', 'runtime': '170 min', 'rating': '8.3', 'votes': '44,968', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt7139936', 'title': 'A Rainy Day in New York', 'rank': '327.', 'year': '2019', 'runtime': '92 min', 'rating': '6.5', 'votes': '44,920', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt7772580', 'title': 'The Perfection', 'rank': '328.', 'year': '2018', 'runtime': '90 min', 'rating': '6.2', 'votes': '44,255', 'genres': 'Drama, Horror, Music            '}, {'movie_id': 'tt8236336', 'title': 'The Report', 'rank': '329.', 'year': '2019', 'runtime': '119 min', 'rating': '7.2', 'votes': '44,236', 'genres': 'Biography, Crime, Drama            '}, {'movie_id': 'tt2850386', 'title': 'The Croods: A New Age', 'rank': '330.', 'year': '2020', 'runtime': '95 min', 'rating': '6.9', 'votes': '43,892', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt5691670', 'title': 'Under the Silver Lake', 'rank': '331.', 'year': '2018', 'runtime': '139 min', 'rating': '6.5', 'votes': '43,571', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt5117670', 'title': 'Peter Rabbit', 'rank': '332.', 'year': '2018', 'runtime': '95 min', 'rating': '6.6', 'votes': '43,560', 'genres': 'Adventure, Comedy, Crime            '}, {'movie_id': 'tt5208252', 'title': 'Operation Finale', 'rank': '333.', 'year': '2018', 'runtime': '122 min', 'rating': '6.6', 'votes': '43,186', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt6772802', 'title': 'Hillbilly Elegy', 'rank': '334.', 'year': '2020', 'runtime': '116 min', 'rating': '6.7', 'votes': '42,770', 'genres': 'Drama            '}, {'movie_id': 'tt8508734', 'title': 'His House', 'rank': '335.', 'year': '2020', 'runtime': '93 min', 'rating': '6.5', 'votes': '42,763', 'genres': 'Drama, Horror, Thriller            '}, {'movie_id': 'tt2235695', 'title': 'Rebecca', 'rank': '336.', 'year': '2020', 'runtime': '123 min', 'rating': '6.0', 'votes': '42,616', 'genres': 'Drama, Mystery, Romance            '}, {'movie_id': 'tt5563334', 'title': 'The Good Liar', 'rank': '337.', 'year': '2019', 'runtime': '109 min', 'rating': '6.7', 'votes': '42,604', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt7985648', 'title': 'The Legend of Tomiris', 'rank': '338.', 'year': '2019', 'runtime': '156 min', 'rating': '6.1', 'votes': '42,330', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt6476140', 'title': 'Serenity', 'rank': '339.', 'year': '2019', 'runtime': '106 min', 'rating': '5.4', 'votes': '41,865', 'genres': 'Drama, Mystery, Thriller            '}, {'movie_id': 'tt0805647', 'title': 'The Witches', 'rank': '340.', 'year': '2020', 'runtime': '106 min', 'rating': '5.3', 'votes': '41,785', 'genres': 'Adventure, Comedy, Family            '}, {'movie_id': 'tt5774450', 'title': 'Summer of 84', 'rank': '341.', 'year': '2018', 'runtime': '105 min', 'rating': '6.7', 'votes': '41,758', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt9426210', 'title': 'Weathering with You', 'rank': '342.', 'year': '2019', 'runtime': '112 min', 'rating': '7.5', 'votes': '41,735', 'genres': 'Animation, Drama, Fantasy            '}, {'movie_id': 'tt3361792', 'title': 'Tolkien', 'rank': '343.', 'year': '2019', 'runtime': '112 min', 'rating': '6.8', 'votes': '41,601', 'genres': 'Biography, Drama, Romance            '}, {'movie_id': 'tt6781982', 'title': 'Night School', 'rank': '344.', 'year': '2018', 'runtime': '111 min', 'rating': '5.6', 'votes': '41,411', 'genres': 'Comedy            '}, {'movie_id': 'tt5001754', 'title': 'Braven', 'rank': '345.', 'year': '2018', 'runtime': '94 min', 'rating': '5.9', 'votes': '41,224', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt11024272', 'title': 'The Babysitter: Killer Queen', 'rank': '346.', 'year': '2020', 'runtime': '101 min', 'rating': '5.8', 'votes': '41,169', 'genres': 'Comedy, Horror            '}, {'movie_id': 'tt8206668', 'title': 'Bad Education', 'rank': '347.', 'year': '2019', 'runtime': '108 min', 'rating': '7.1', 'votes': '40,668', 'genres': 'Biography, Comedy, Crime            '}, {'movie_id': 'tt6259380', 'title': 'Code 8', 'rank': '348.', 'year': '2019', 'runtime': '98 min', 'rating': '6.1', 'votes': '40,516', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt9619798', 'title': 'The Wrong Missy', 'rank': '349.', 'year': '2020', 'runtime': '90 min', 'rating': '5.7', 'votes': '40,329', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt10620868', 'title': '#Alive', 'rank': '350.', 'year': '2020', 'runtime': '98 min', 'rating': '6.3', 'votes': '40,291', 'genres': 'Action, Drama, Horror            '}, {'movie_id': 'tt1620981', 'title': 'The Addams Family', 'rank': '351.', 'year': '2019', 'runtime': '86 min', 'rating': '5.8', 'votes': '40,282', 'genres': 'Animation, Comedy, Family            '}, {'movie_id': 'tt8522006', 'title': 'Happiest Season', 'rank': '352.', 'year': '2020', 'runtime': '102 min', 'rating': '6.6', 'votes': '40,156', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt2395469', 'title': 'Gully Boy', 'rank': '353.', 'year': '2019', 'runtime': '154 min', 'rating': '7.9', 'votes': '40,122', 'genres': 'Drama, Music, Romance            '}, {'movie_id': 'tt6182908', 'title': 'Smallfoot', 'rank': '354.', 'year': '2018', 'runtime': '96 min', 'rating': '6.6', 'votes': '40,022', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt6803046', 'title': 'The Vast of Night', 'rank': '355.', 'year': '2019', 'runtime': '91 min', 'rating': '6.7', 'votes': '40,012', 'genres': 'Drama, Mystery, Sci-Fi            '}, {'movie_id': 'tt7212754', 'title': 'Ludo', 'rank': '356.', 'year': '2020', 'runtime': '150 min', 'rating': '7.6', 'votes': '39,911', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt9354842', 'title': 'To All the Boys: P.S. I Still Love You', 'rank': '357.', 'year': '2020', 'runtime': '101 min', 'rating': '6.0', 'votes': '39,754', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt8201170', 'title': 'The Perfect Date', 'rank': '358.', 'year': '2019', 'runtime': '89 min', 'rating': '5.8', 'votes': '39,730', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt3120280', 'title': 'Sierra Burgess Is a Loser', 'rank': '359.', 'year': '2018', 'runtime': '105 min', 'rating': '5.8', 'votes': '39,404', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt7008872', 'title': 'Boy Erased', 'rank': '360.', 'year': '2018', 'runtime': '115 min', 'rating': '6.9', 'votes': '39,230', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt10039344', 'title': 'Countdown', 'rank': '361.', 'year': '2019', 'runtime': '90 min', 'rating': '5.4', 'votes': '39,220', 'genres': 'Horror, Thriller            '}, {'movie_id': 'tt4154916', 'title': 'Replicas', 'rank': '362.', 'year': '2018', 'runtime': '107 min', 'rating': '5.4', 'votes': '39,023', 'genres': 'Drama, Sci-Fi, Thriller            '}, {'movie_id': 'tt1563742', 'title': 'Overboard', 'rank': '363.', 'year': '2018', 'runtime': '112 min', 'rating': '6.0', 'votes': '38,978', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt6324278', 'title': 'Abominable', 'rank': '364.', 'year': '2019', 'runtime': '97 min', 'rating': '7.0', 'votes': '38,927', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt7533152', 'title': 'The Boy Who Harnessed the Wind', 'rank': '365.', 'year': '2019', 'runtime': '113 min', 'rating': '7.6', 'votes': '38,883', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt7557108', 'title': 'Saint Maud', 'rank': '366.', 'year': '2019', 'runtime': '84 min', 'rating': '6.7', 'votes': '38,310', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt4537896', 'title': 'White Boy Rick', 'rank': '367.', 'year': '2018', 'runtime': '111 min', 'rating': '6.5', 'votes': '38,306', 'genres': 'Crime, Drama            '}, {'movie_id': 'tt9683478', 'title': 'The Half of It', 'rank': '368.', 'year': '2020', 'runtime': '104 min', 'rating': '6.9', 'votes': '38,303', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt6107548', 'title': 'Late Night', 'rank': '369.', 'year': '2019', 'runtime': '102 min', 'rating': '6.5', 'votes': '38,121', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt7339248', 'title': 'The Art of Self-Defense', 'rank': '370.', 'year': '2019', 'runtime': '104 min', 'rating': '6.6', 'votes': '38,087', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt5797184', 'title': 'Escape from Pretoria', 'rank': '371.', 'year': '2020', 'runtime': '106 min', 'rating': '6.8', 'votes': '37,913', 'genres': 'Biography, Crime, Mystery            '}, {'movie_id': 'tt8151874', 'title': 'Honey Boy', 'rank': '372.', 'year': '2019', 'runtime': '94 min', 'rating': '7.2', 'votes': '37,908', 'genres': 'Drama            '}, {'movie_id': 'tt1833116', 'title': 'The Informer', 'rank': '373.', 'year': '2019', 'runtime': '113 min', 'rating': '6.6', 'votes': '37,854', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt3385524', 'title': 'Stan & Ollie', 'rank': '374.', 'year': '2018', 'runtime': '98 min', 'rating': '7.2', 'votes': '37,802', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt5057140', 'title': 'Hold the Dark', 'rank': '375.', 'year': '2018', 'runtime': '125 min', 'rating': '5.6', 'votes': '37,761', 'genres': 'Action, Drama, Thriller            '}, {'movie_id': 'tt5619332', 'title': 'Life of the Party', 'rank': '376.', 'year': '2018', 'runtime': '105 min', 'rating': '5.6', 'votes': '37,575', 'genres': 'Comedy            '}, {'movie_id': 'tt1255919', 'title': 'Holmes & Watson', 'rank': '377.', 'year': '2018', 'runtime': '90 min', 'rating': '3.9', 'votes': '36,994', 'genres': 'Comedy, Crime, Mystery            '}, {'movie_id': 'tt5918982', 'title': 'Possessor', 'rank': '378.', 'year': '2020', 'runtime': '103 min', 'rating': '6.5', 'votes': '36,975', 'genres': 'Horror, Mystery, Sci-Fi            '}, {'movie_id': 'tt6697582', 'title': 'Arif V 216', 'rank': '379.', 'year': '2018', 'runtime': '125 min', 'rating': '7.0', 'votes': '36,943', 'genres': 'Comedy, Fantasy, Music            '}, {'movie_id': 'tt5580266', 'title': 'The Hate U Give', 'rank': '380.', 'year': '2018', 'runtime': '133 min', 'rating': '7.5', 'votes': '36,834', 'genres': 'Crime, Drama            '}, {'movie_id': 'tt5294518', 'title': 'Eli', 'rank': '381.', 'year': '2019', 'runtime': '98 min', 'rating': '5.8', 'votes': '36,714', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt4827558', 'title': 'High Life', 'rank': '382.', 'year': '2018', 'runtime': '113 min', 'rating': '5.8', 'votes': '36,488', 'genres': 'Adventure, Drama, Horror            '}, {'movie_id': 'tt8781414', 'title': 'Freaks', 'rank': '383.', 'year': '2018', 'runtime': '105 min', 'rating': '6.7', 'votes': '36,019', 'genres': 'Drama, Mystery, Sci-Fi            '}, {'movie_id': 'tt1464763', 'title': 'Mute', 'rank': '384.', 'year': '2018', 'runtime': '126 min', 'rating': '5.4', 'votes': '35,869', 'genres': 'Mystery, Sci-Fi, Thriller            '}, {'movie_id': 'tt6938828', 'title': "At Eternity's Gate", 'rank': '385.', 'year': '2018', 'runtime': '111 min', 'rating': '6.9', 'votes': '35,691', 'genres': 'Biography, Drama, History            '}, {'movie_id': 'tt3833480', 'title': 'The Outpost', 'rank': '386.', 'year': '2019', 'runtime': '123 min', 'rating': '6.8', 'votes': '35,201', 'genres': 'Action, Drama, History            '}, {'movie_id': 'tt4964788', 'title': 'Everybody Knows', 'rank': '387.', 'year': '2018', 'runtime': '133 min', 'rating': '6.9', 'votes': '35,183', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt7725596', 'title': 'Badhaai Ho', 'rank': '388.', 'year': '2018', 'runtime': '124 min', 'rating': '7.9', 'votes': '35,017', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt4669788', 'title': 'On the Basis of Sex', 'rank': '389.', 'year': '2018', 'runtime': '120 min', 'rating': '7.1', 'votes': '34,969', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt10350626', 'title': 'Gunjan Saxena: The Kargil Girl', 'rank': '390.', 'year': '2020', 'runtime': '112 min', 'rating': '5.6', 'votes': '34,907', 'genres': 'Action, Biography, Drama            '}, {'movie_id': 'tt5690360', 'title': 'Slender Man', 'rank': '391.', 'year': '2018', 'runtime': '93 min', 'rating': '3.2', 'votes': '34,888', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt7608028', 'title': 'The Open House', 'rank': '392.', 'year': '2018', 'runtime': '94 min', 'rating': '3.3', 'votes': '34,873', 'genres': 'Horror, Thriller            '}, {'movie_id': 'tt3256226', 'title': 'IO', 'rank': '393.', 'year': '2019', 'runtime': '96 min', 'rating': '4.7', 'votes': '34,675', 'genres': 'Adventure, Drama, Sci-Fi            '}, {'movie_id': 'tt1072748', 'title': 'Winchester', 'rank': '394.', 'year': '2018', 'runtime': '99 min', 'rating': '5.4', 'votes': '34,670', 'genres': 'Drama, Fantasy, Horror            '}, {'movie_id': 'tt8108202', 'title': 'Stree', 'rank': '395.', 'year': '2018', 'runtime': '128 min', 'rating': '7.5', 'votes': '34,529', 'genres': 'Comedy, Horror            '}, {'movie_id': 'tt4878482', 'title': "Dumplin'", 'rank': '396.', 'year': '2018', 'runtime': '110 min', 'rating': '6.5', 'votes': '34,483', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt7280898', 'title': '22 July', 'rank': '397.', 'year': '2018', 'runtime': '143 min', 'rating': '6.8', 'votes': '34,478', 'genres': 'Crime, Drama, History            '}, {'movie_id': 'tt2639336', 'title': 'Greta', 'rank': '398.', 'year': '2018', 'runtime': '98 min', 'rating': '6.0', 'votes': '34,429', 'genres': 'Drama, Mystery, Thriller            '}, {'movie_id': 'tt8108268', 'title': 'The Tashkent Files', 'rank': '399.', 'year': '2019', 'runtime': '134 min', 'rating': '8.1', 'votes': '34,317', 'genres': 'Drama, Mystery, Thriller            '}, {'movie_id': 'tt10324144', 'title': 'Article 15', 'rank': '400.', 'year': '2019', 'runtime': '130 min', 'rating': '8.1', 'votes': '34,050', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt6141246', 'title': 'The Aeronauts', 'rank': '401.', 'year': '2019', 'runtime': '100 min', 'rating': '6.6', 'votes': '34,028', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt6802308', 'title': 'The 15:17 to Paris', 'rank': '402.', 'year': '2018', 'runtime': '94 min', 'rating': '5.3', 'votes': '34,014', 'genres': 'Biography, Drama, Thriller            '}, {'movie_id': 'tt8983202', 'title': 'Kabir Singh', 'rank': '403.', 'year': '2019', 'runtime': '173 min', 'rating': '7.0', 'votes': '33,918', 'genres': 'Action, Drama, Romance            '}, {'movie_id': 'tt5523010', 'title': 'The Nutcracker and the Four Realms', 'rank': '404.', 'year': '2018', 'runtime': '99 min', 'rating': '5.5', 'votes': '33,912', 'genres': 'Adventure, Family, Fantasy            '}, {'movie_id': 'tt4761916', 'title': 'Unfriended: Dark Web', 'rank': '405.', 'year': '2018', 'runtime': '92 min', 'rating': '6.0', 'votes': '33,839', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt8633462', 'title': 'Quo Vadis, Aida?', 'rank': '406.', 'year': '2020', 'runtime': '101 min', 'rating': '8.0', 'votes': '33,815', 'genres': 'Drama, History, War            '}, {'movie_id': 'tt10530176', 'title': 'The Call', 'rank': '407.', 'year': '2020', 'runtime': '112 min', 'rating': '7.1', 'votes': '33,611', 'genres': 'Crime, Horror, Mystery            '}, {'movie_id': 'tt6195094', 'title': 'Incident in a Ghostland', 'rank': '408.', 'year': '2018', 'runtime': '91 min', 'rating': '6.4', 'votes': '33,447', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt9806192', 'title': 'I Lost My Body', 'rank': '409.', 'year': '2019', 'runtime': '81 min', 'rating': '7.5', 'votes': '33,397', 'genres': 'Animation, Drama, Fantasy            '}, {'movie_id': 'tt8850222', 'title': 'Peninsula', 'rank': '410.', 'year': '2020', 'runtime': '116 min', 'rating': '5.5', 'votes': '33,347', 'genres': 'Action, Horror, Thriller            '}, {'movie_id': 'tt5935704', 'title': 'Padmaavat', 'rank': '411.', 'year': '2018', 'runtime': '164 min', 'rating': '7.0', 'votes': '33,226', 'genres': 'Drama, History, Romance            '}, {'movie_id': 'tt6865690', 'title': 'The Professor', 'rank': '412.', 'year': '2018', 'runtime': '90 min', 'rating': '6.7', 'votes': '33,218', 'genres': 'Comedy, Drama            '}, {'movie_id': 'tt9398640', 'title': 'Between Two Ferns: The Movie', 'rank': '413.', 'year': '2019', 'runtime': '82 min', 'rating': '6.1', 'votes': '33,205', 'genres': 'Comedy            '}, {'movie_id': 'tt2076298', 'title': 'Ip Man 4: The Finale', 'rank': '414.', 'year': '2019', 'runtime': '107 min', 'rating': '7.0', 'votes': '32,815', 'genres': 'Action, Biography, Drama            '}, {'movie_id': 'tt10003008', 'title': 'The Rental', 'rank': '415.', 'year': '2020', 'runtime': '88 min', 'rating': '5.7', 'votes': '32,793', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt7485048', 'title': 'Super 30', 'rank': '416.', 'year': '2019', 'runtime': '154 min', 'rating': '7.9', 'votes': '32,769', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt4073790', 'title': 'The Darkest Minds', 'rank': '417.', 'year': '2018', 'runtime': '104 min', 'rating': '5.7', 'votes': '32,745', 'genres': 'Action, Adventure, Drama            '}, {'movie_id': 'tt5198068', 'title': 'Wolfwalkers', 'rank': '418.', 'year': '2020', 'runtime': '103 min', 'rating': '8.0', 'votes': '32,741', 'genres': 'Animation, Adventure, Family            '}, {'movie_id': 'tt8108274', 'title': 'Tanhaji: The Unsung Warrior', 'rank': '419.', 'year': '2020', 'runtime': '135 min', 'rating': '7.5', 'votes': '32,685', 'genres': 'Action, Biography, Drama            '}, {'movie_id': 'tt9606374', 'title': 'On the Rocks', 'rank': '420.', 'year': '2020', 'runtime': '96 min', 'rating': '6.4', 'votes': '32,652', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt8361028', 'title': 'Cam', 'rank': '421.', 'year': '2018', 'runtime': '94 min', 'rating': '5.9', 'votes': '32,624', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt9900782', 'title': 'Kaithi', 'rank': '422.', 'year': '2019', 'runtime': '145 min', 'rating': '8.5', 'votes': '32,569', 'genres': 'Action, Adventure, Crime            '}, {'movie_id': 'tt10362466', 'title': 'After We Collided', 'rank': '423.', 'year': '2020', 'runtime': '105 min', 'rating': '5.0', 'votes': '32,474', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt9784456', 'title': 'The Kissing Booth 2', 'rank': '424.', 'year': '2020', 'runtime': '134 min', 'rating': '5.7', 'votes': '32,438', 'genres': 'Comedy, Romance            '}, {'movie_id': 'tt6513656', 'title': 'Escape Plan 2: Hades', 'rank': '425.', 'year': '2018', 'runtime': '96 min', 'rating': '3.8', 'votes': '32,411', 'genres': 'Action, Crime, Mystery            '}, {'movie_id': 'tt3907584', 'title': 'All the Bright Places', 'rank': '426.', 'year': '2020', 'runtime': '107 min', 'rating': '6.5', 'votes': '32,326', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt10612922', 'title': 'One Night in Miami...', 'rank': '427.', 'year': '2020', 'runtime': '114 min', 'rating': '7.1', 'votes': '32,298', 'genres': 'Drama            '}, {'movie_id': 'tt1478839', 'title': 'The Art of Racing in the Rain', 'rank': '428.', 'year': '2019', 'runtime': '109 min', 'rating': '7.5', 'votes': '32,207', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt8722346', 'title': 'Queen & Slim', 'rank': '429.', 'year': '2019', 'runtime': '132 min', 'rating': '7.1', 'votes': '32,070', 'genres': 'Crime, Drama, Romance            '}, {'movie_id': 'tt4799066', 'title': 'Midnight Sun', 'rank': '430.', 'year': '2018', 'runtime': '91 min', 'rating': '6.6', 'votes': '32,018', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt4357394', 'title': 'Tau', 'rank': '431.', 'year': '2018', 'runtime': '97 min', 'rating': '5.8', 'votes': '31,974', 'genres': 'Sci-Fi, Thriller            '}, {'movie_id': 'tt9016974', 'title': 'Synchronic', 'rank': '432.', 'year': '2019', 'runtime': '102 min', 'rating': '6.2', 'votes': '31,844', 'genres': 'Crime, Drama, Horror            '}, {'movie_id': 'tt1285009', 'title': 'The Strangers: Prey at Night', 'rank': '433.', 'year': '2018', 'runtime': '85 min', 'rating': '5.2', 'votes': '31,777', 'genres': 'Horror            '}, {'movie_id': 'tt4986098', 'title': 'The Titan', 'rank': '434.', 'year': '2018', 'runtime': '97 min', 'rating': '4.8', 'votes': '31,725', 'genres': 'Drama, Mystery, Romance            '}, {'movie_id': 'tt7019842', 'title': '96', 'rank': '435.', 'year': '2018', 'runtime': '158 min', 'rating': '8.5', 'votes': '31,611', 'genres': 'Drama, Romance            '}, {'movie_id': 'tt4068576', 'title': 'The Nightingale', 'rank': '436.', 'year': '2018', 'runtime': '136 min', 'rating': '7.3', 'votes': '31,567', 'genres': 'Adventure, Drama, Thriller            '}, {'movie_id': 'tt9086228', 'title': 'Gretel & Hansel', 'rank': '437.', 'year': '2020', 'runtime': '87 min', 'rating': '5.5', 'votes': '31,384', 'genres': 'Fantasy, Horror, Mystery            '}, {'movie_id': 'tt7547410', 'title': 'Dora and the Lost City of Gold', 'rank': '438.', 'year': '2019', 'runtime': '102 min', 'rating': '6.1', 'votes': '31,381', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt7098658', 'title': 'Raazi', 'rank': '439.', 'year': '2018', 'runtime': '138 min', 'rating': '7.7', 'votes': '31,359', 'genres': 'Action, Drama, Thriller            '}, {'movie_id': 'tt7721800', 'title': 'Bharat', 'rank': '440.', 'year': '2019', 'runtime': '150 min', 'rating': '4.7', 'votes': '31,275', 'genres': 'Action, Comedy, Drama            '}, {'movie_id': 'tt7242142', 'title': 'Blindspotting', 'rank': '441.', 'year': '2018', 'runtime': '95 min', 'rating': '7.4', 'votes': '31,158', 'genres': 'Comedy, Crime, Drama            '}, {'movie_id': 'tt8242084', 'title': 'My Spy', 'rank': '442.', 'year': '2020', 'runtime': '99 min', 'rating': '6.3', 'votes': '31,150', 'genres': 'Action, Comedy            '}, {'movie_id': 'tt12749596', 'title': 'Host', 'rank': '443.', 'year': '2020', 'runtime': '57 min', 'rating': '6.5', 'votes': '31,071', 'genres': 'Horror, Mystery            '}, {'movie_id': 'tt9196192', 'title': 'Cuties', 'rank': '444.', 'year': '2020', 'runtime': '96 min', 'rating': '3.5', 'votes': '30,906', 'genres': 'Drama            '}, {'movie_id': 'tt7605074', 'title': 'The Wandering Earth', 'rank': '445.', 'year': '2019', 'runtime': '125 min', 'rating': '5.9', 'votes': '30,891', 'genres': 'Action, Adventure, Sci-Fi            '}, {'movie_id': 'tt3317234', 'title': 'Game Over, Man!', 'rank': '446.', 'year': '2018', 'runtime': '101 min', 'rating': '5.4', 'votes': '30,880', 'genres': 'Action, Comedy            '}, {'movie_id': 'tt5501104', 'title': 'Border', 'rank': '447.', 'year': '2018', 'runtime': '110 min', 'rating': '7.0', 'votes': '30,648', 'genres': 'Crime, Drama, Fantasy            '}, {'movie_id': 'tt3721964', 'title': 'Gringo', 'rank': '448.', 'year': '2018', 'runtime': '111 min', 'rating': '6.1', 'votes': '30,595', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt6987770', 'title': 'Destination Wedding', 'rank': '449.', 'year': '2018', 'runtime': '87 min', 'rating': '6.0', 'votes': '30,524', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt5867314', 'title': 'The Empty Man', 'rank': '450.', 'year': '2020', 'runtime': '137 min', 'rating': '6.2', 'votes': '30,464', 'genres': 'Horror, Mystery, Thriller            '}, {'movie_id': 'tt11057644', 'title': 'The Christmas Chronicles: Part Two', 'rank': '451.', 'year': '2020', 'runtime': '112 min', 'rating': '6.0', 'votes': '30,456', 'genres': 'Adventure, Comedy, Family            '}, {'movie_id': 'tt10521092', 'title': 'The Forgotten Battle', 'rank': '452.', 'year': '2020', 'runtime': '124 min', 'rating': '7.1', 'votes': '30,270', 'genres': 'Drama, War            '}, {'movie_id': 'tt6095472', 'title': 'The Angry Birds Movie 2', 'rank': '453.', 'year': '2019', 'runtime': '97 min', 'rating': '6.4', 'votes': '30,205', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt6535880', 'title': 'Haunt', 'rank': '454.', 'year': '2019', 'runtime': '92 min', 'rating': '6.3', 'votes': '30,070', 'genres': 'Horror, Thriller            '}, {'movie_id': 'tt7137380', 'title': 'Destroyer', 'rank': '455.', 'year': '2018', 'runtime': '121 min', 'rating': '6.2', 'votes': '30,022', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt6218358', 'title': 'Calibre', 'rank': '456.', 'year': '2018', 'runtime': '101 min', 'rating': '6.8', 'votes': '29,935', 'genres': 'Drama, Thriller            '}, {'movie_id': 'tt4995776', 'title': 'The Red Sea Diving Resort', 'rank': '457.', 'year': '2019', 'runtime': '129 min', 'rating': '6.6', 'votes': '29,840', 'genres': 'Drama, History, Thriller            '}, {'movie_id': 'tt9477520', 'title': 'Asuran', 'rank': '458.', 'year': '2019', 'runtime': '141 min', 'rating': '8.4', 'votes': '29,788', 'genres': 'Action, Drama            '}, {'movie_id': 'tt6436726', 'title': '7500', 'rank': '459.', 'year': '2019', 'runtime': '93 min', 'rating': '6.3', 'votes': '29,562', 'genres': 'Action, Drama, Thriller            '}, {'movie_id': 'tt5929754', 'title': 'Wildlife', 'rank': '460.', 'year': '2018', 'runtime': '105 min', 'rating': '6.8', 'votes': '29,373', 'genres': 'Drama            '}, {'movie_id': 'tt6285944', 'title': 'The Banker', 'rank': '461.', 'year': '2020', 'runtime': '120 min', 'rating': '7.3', 'votes': '29,107', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt10964430', 'title': 'Thappad', 'rank': '462.', 'year': '2020', 'runtime': '142 min', 'rating': '7.0', 'votes': '28,756', 'genres': 'Drama            '}, {'movie_id': 'tt3089630', 'title': 'Artemis Fowl', 'rank': '463.', 'year': '2020', 'runtime': '95 min', 'rating': '4.2', 'votes': '28,620', 'genres': 'Adventure, Family, Fantasy            '}, {'movie_id': 'tt6857166', 'title': 'Book Club', 'rank': '464.', 'year': '2018', 'runtime': '104 min', 'rating': '6.1', 'votes': '28,608', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt7772582', 'title': 'Never Rarely Sometimes Always', 'rank': '465.', 'year': '2020', 'runtime': '101 min', 'rating': '7.4', 'votes': '28,606', 'genres': 'Drama            '}, {'movie_id': 'tt8954732', 'title': 'The Princess Switch', 'rank': '466.', 'year': '2018', 'runtime': '101 min', 'rating': '6.0', 'votes': '28,559', 'genres': 'Comedy, Drama, Family            '}, {'movie_id': 'tt6269368', 'title': 'The Clovehitch Killer', 'rank': '467.', 'year': '2018', 'runtime': '109 min', 'rating': '6.5', 'votes': '28,542', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt1308728', 'title': 'The Happytime Murders', 'rank': '468.', 'year': '2018', 'runtime': '91 min', 'rating': '5.4', 'votes': '28,517', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt8130968', 'title': 'Badla', 'rank': '469.', 'year': '2019', 'runtime': '118 min', 'rating': '7.8', 'votes': '28,494', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt7946422', 'title': 'Prospect', 'rank': '470.', 'year': '2018', 'runtime': '100 min', 'rating': '6.3', 'votes': '28,387', 'genres': 'Adventure, Drama, Sci-Fi            '}, {'movie_id': 'tt7634968', 'title': 'What Men Want', 'rank': '471.', 'year': '2019', 'runtime': '117 min', 'rating': '5.3', 'votes': '28,344', 'genres': 'Comedy, Fantasy, Romance            '}, {'movie_id': 'tt7961060', 'title': 'Dragon Ball Super: Broly', 'rank': '472.', 'year': '2018', 'runtime': '100 min', 'rating': '7.7', 'votes': '28,218', 'genres': 'Animation, Action, Adventure            '}, {'movie_id': 'tt6527426', 'title': 'Zero', 'rank': '473.', 'year': '2018', 'runtime': '164 min', 'rating': '5.2', 'votes': '28,114', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt2126357', 'title': 'Second Act', 'rank': '474.', 'year': '2018', 'runtime': '103 min', 'rating': '5.8', 'votes': '28,000', 'genres': 'Comedy, Drama, Romance            '}, {'movie_id': 'tt9354944', 'title': 'Jexi', 'rank': '475.', 'year': '2019', 'runtime': '84 min', 'rating': '6.0', 'votes': '27,910', 'genres': 'Comedy, Romance, Sci-Fi            '}, {'movie_id': 'tt2762506', 'title': 'Bacurau', 'rank': '476.', 'year': '2019', 'runtime': '131 min', 'rating': '7.3', 'votes': '27,891', 'genres': 'Action, Adventure, Horror            '}, {'movie_id': 'tt6288124', 'title': "Don't Worry, He Won't Get Far on Foot", 'rank': '477.', 'year': '2018', 'runtime': '114 min', 'rating': '6.8', 'votes': '27,588', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt10161886', 'title': 'The Prom', 'rank': '478.', 'year': '2020', 'runtime': '130 min', 'rating': '5.9', 'votes': '27,523', 'genres': 'Comedy, Drama, Musical            '}, {'movie_id': 'tt4648786', 'title': 'Harriet', 'rank': '479.', 'year': '2019', 'runtime': '125 min', 'rating': '6.7', 'votes': '27,477', 'genres': 'Action, Biography, Drama            '}, {'movie_id': 'tt10065694', 'title': 'Antebellum', 'rank': '480.', 'year': '2020', 'runtime': '105 min', 'rating': '5.8', 'votes': '27,429', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt9691136', 'title': 'Shadow in the Cloud', 'rank': '481.', 'year': '2020', 'runtime': '83 min', 'rating': '5.0', 'votes': '27,364', 'genres': 'Action, Horror, War            '}, {'movie_id': 'tt6768578', 'title': 'Dogman', 'rank': '482.', 'year': '2018', 'runtime': '103 min', 'rating': '7.2', 'votes': '27,280', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt9072352', 'title': 'Relic', 'rank': '483.', 'year': '2020', 'runtime': '89 min', 'rating': '6.0', 'votes': '27,276', 'genres': 'Drama, Horror, Mystery            '}, {'movie_id': 'tt9252468', 'title': 'Mosul', 'rank': '484.', 'year': '2019', 'runtime': '101 min', 'rating': '7.1', 'votes': '27,252', 'genres': 'Action, Drama, War            '}, {'movie_id': 'tt5144174', 'title': 'The Dry', 'rank': '485.', 'year': '2020', 'runtime': '117 min', 'rating': '6.8', 'votes': '27,209', 'genres': 'Crime, Drama, Mystery            '}, {'movie_id': 'tt9614460', 'title': 'Chhapaak', 'rank': '486.', 'year': '2020', 'runtime': '120 min', 'rating': '5.3', 'votes': '27,207', 'genres': 'Biography, Drama            '}, {'movie_id': 'tt7748244', 'title': 'Mortal World', 'rank': '487.', 'year': '2018', 'runtime': '107 min', 'rating': '7.6', 'votes': '27,186', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt3152592', 'title': 'Scoob!', 'rank': '488.', 'year': '2020', 'runtime': '93 min', 'rating': '5.6', 'votes': '27,170', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt9340860', 'title': 'Let Him Go', 'rank': '489.', 'year': '2020', 'runtime': '113 min', 'rating': '6.7', 'votes': '27,145', 'genres': 'Crime, Drama, Thriller            '}, {'movie_id': 'tt7329656', 'title': '47 Meters Down: Uncaged', 'rank': '490.', 'year': '2019', 'runtime': '90 min', 'rating': '5.0', 'votes': '26,919', 'genres': 'Adventure, Drama, Horror            '}, {'movie_id': 'tt0859635', 'title': 'Super Troopers 2', 'rank': '491.', 'year': '2018', 'runtime': '99 min', 'rating': '6.0', 'votes': '26,897', 'genres': 'Comedy, Crime, Mystery            '}, {'movie_id': 'tt4504044', 'title': 'The Prodigy', 'rank': '492.', 'year': '2019', 'runtime': '92 min', 'rating': '5.9', 'votes': '26,802', 'genres': 'Fantasy, Horror, Mystery            '}, {'movie_id': 'tt3612126', 'title': 'The Grudge', 'rank': '493.', 'year': '2020', 'runtime': '94 min', 'rating': '4.4', 'votes': '26,680', 'genres': 'Fantasy, Horror            '}, {'movie_id': 'tt7218518', 'title': 'Pad Man', 'rank': '494.', 'year': '2018', 'runtime': '140 min', 'rating': '7.9', 'votes': '26,569', 'genres': 'Biography, Comedy, Drama            '}, {'movie_id': 'tt6116856', 'title': 'The Night Comes for Us', 'rank': '495.', 'year': '2018', 'runtime': '121 min', 'rating': '6.9', 'votes': '26,553', 'genres': 'Action, Thriller            '}, {'movie_id': 'tt6348138', 'title': 'Missing Link', 'rank': '496.', 'year': '2019', 'runtime': '93 min', 'rating': '6.7', 'votes': '26,508', 'genres': 'Animation, Adventure, Comedy            '}, {'movie_id': 'tt2011311', 'title': 'The Outsider', 'rank': '497.', 'year': '2018', 'runtime': '120 min', 'rating': '6.2', 'votes': '26,458', 'genres': 'Action, Crime, Drama            '}, {'movie_id': 'tt8851668', 'title': 'The Lovebirds', 'rank': '498.', 'year': '2020', 'runtime': '86 min', 'rating': '6.1', 'votes': '26,372', 'genres': 'Action, Comedy, Crime            '}, {'movie_id': 'tt6521876', 'title': 'Jay and Silent Bob Reboot', 'rank': '499.', 'year': '2019', 'runtime': '105 min', 'rating': '5.6', 'votes': '26,148', 'genres': 'Action, Adventure, Comedy            '}, {'movie_id': 'tt8652728', 'title': 'Waves', 'rank': '500.', 'year': '2019', 'runtime': '135 min', 'rating': '7.5', 'votes': '26,128', 'genres': 'Drama, Romance, Sport            '}]
    ================================================================


Based on the the parameters provided, (2018, 2020, 500), we can see that the top 500 movies were successfully scraped from the IMDb ranking list.

# Importing the given dataset "Movies.csv" to Pandas DataFrame called df1


```python
df1 = pd.read_csv(r'C:\Users\patki\Movies.csv')
```

# Import the scraped data from the IMDb_TopVoted.csv file to Pandas DataFrame called df2


```python
df2 = pd.read_csv(r'C:\Users\patki\IMDb_TopVoted.csv', encoding = "ISO-8859-1")
```

# Data cleansing and transformation for df2.

### Data Exploration:


```python
# Checking the data types for each column in df2:
df2.dtypes
```




    movie_id     object
    title        object
    rank        float64
    year         object
    runtime      object
    rating      float64
    votes        object
    genres       object
    dtype: object




```python
# Checking df2 for null values:
df2.isnull().sum()
```




    movie_id    0
    title       0
    rank        0
    year        0
    runtime     0
    rating      0
    votes       0
    genres      0
    dtype: int64



We can see that there are no null values in any of the columns in df2.


```python
# Using '.describe()' to count the unique values in each column. This is to ensure that all the observations in 'movie_id', 'title'  'rank' are unique, and that the 'year' column only has 3 distinct values (2018, 2019, or 2020).
# For columns with 'object' data type:
df2.describe(include=object)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>movie_id</th>
      <th>title</th>
      <th>year</th>
      <th>runtime</th>
      <th>votes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>500</td>
      <td>500</td>
      <td>500</td>
      <td>500</td>
      <td>500</td>
      <td>500</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>500</td>
      <td>500</td>
      <td>4</td>
      <td>86</td>
      <td>498</td>
      <td>138</td>
    </tr>
    <tr>
      <th>top</th>
      <td>tt7286456</td>
      <td>Joker</td>
      <td>2018</td>
      <td>90 min</td>
      <td>106,127</td>
      <td>Action, Adventure, Sci-Fi</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>1</td>
      <td>1</td>
      <td>202</td>
      <td>16</td>
      <td>2</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Counting unique values in the 'rank' column:
df2['rank'].unique().size
```




    500



We can see that 'movie_id', 'title', and 'rank' columns correctly have 500 unique values. However, we also see that the 'year' column has **4** unique values instead of 3. I explore the 'year' column to investigate further:


```python
df2.year.unique()
```




    array(['2019', '2018', '2020', 'V2020'], dtype=object)



The 'year' column has an incorrect value of 'V2020' which will have to be replaced. 

### Data cleansing and transformation:

In order to convert 'year', 'runtime' and 'votes' into integer data type, we will first have to clean the data in order to:
- Replace 'V2020' in the 'year' column with '2020'.
- Remove the 'min' characters from each observation in the 'runtime' column. 
- Remove the commas from each observation in the 'votes' column.


```python
# Removing non-numeric characters from 'year' and 'runtime' columns
df2['year'] = df2['year'].replace('V2020', '2020')
df2.runtime = pd.to_numeric(df2.runtime.str.replace("min", ""))

# Removing commas from 'votes' columns

df2.votes = df2.votes.str.replace(",", "")

# rank, year, runtime, and votes should have a numeric integer data type.

cols = ['rank', 'year', 'runtime', 'votes']
df2[cols]=df2[cols].astype(int)

df2.dtypes
```




    movie_id     object
    title        object
    rank          int32
    year          int32
    runtime       int32
    rating      float64
    votes         int32
    genres       object
    dtype: object



# 	Enrich the given dataset (df1) by merging it to the scraped data (df2).


```python
# Merge the two dataframes to one dataframe called df.
df = pd.merge(df1, df2)
```

# Rearrange the dataset fields to be listed in the following order: 
 movie_id, rank, votes, title, originalTitle, year, rating, titleType, isAdult, runtime,  genres


```python
# Rearrange the dataset fields.

df = df[['movie_id','rank', 'votes', 'title', 'originalTitle', 'year', 'rating', 'titleType', 'isAdult', 'runtime', 'genres']]


#ordering the dataset by 'rank'

df = df.sort_values(by=['rank'])

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>movie_id</th>
      <th>rank</th>
      <th>votes</th>
      <th>title</th>
      <th>originalTitle</th>
      <th>year</th>
      <th>rating</th>
      <th>titleType</th>
      <th>isAdult</th>
      <th>runtime</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt7286456</td>
      <td>1</td>
      <td>1271181</td>
      <td>Joker</td>
      <td>Joker</td>
      <td>2019</td>
      <td>8.4</td>
      <td>movie</td>
      <td>0</td>
      <td>122</td>
      <td>Crime, Drama, Thriller</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt4154796</td>
      <td>2</td>
      <td>1121459</td>
      <td>Avengers: Endgame</td>
      <td>Avengers: Endgame</td>
      <td>2019</td>
      <td>8.4</td>
      <td>movie</td>
      <td>0</td>
      <td>181</td>
      <td>Action, Adventure, Drama</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt4154756</td>
      <td>3</td>
      <td>1073326</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>8.4</td>
      <td>movie</td>
      <td>0</td>
      <td>149</td>
      <td>Action, Adventure, Sci-Fi</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt6751668</td>
      <td>4</td>
      <td>794414</td>
      <td>Parasite</td>
      <td>Gisaengchung</td>
      <td>2019</td>
      <td>8.5</td>
      <td>movie</td>
      <td>0</td>
      <td>132</td>
      <td>Drama, Thriller</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt1825683</td>
      <td>5</td>
      <td>767098</td>
      <td>Black Panther</td>
      <td>Black Panther</td>
      <td>2018</td>
      <td>7.3</td>
      <td>movie</td>
      <td>0</td>
      <td>134</td>
      <td>Action, Adventure, Sci-Fi</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>483</th>
      <td>tt6116856</td>
      <td>495</td>
      <td>26553</td>
      <td>The Night Comes for Us</td>
      <td>The Night Comes for Us</td>
      <td>2018</td>
      <td>6.9</td>
      <td>movie</td>
      <td>0</td>
      <td>121</td>
      <td>Action, Thriller</td>
    </tr>
    <tr>
      <th>485</th>
      <td>tt6348138</td>
      <td>496</td>
      <td>26508</td>
      <td>Missing Link</td>
      <td>Missing Link</td>
      <td>2019</td>
      <td>6.7</td>
      <td>movie</td>
      <td>0</td>
      <td>93</td>
      <td>Animation, Adventure, Comedy</td>
    </tr>
    <tr>
      <th>491</th>
      <td>tt2011311</td>
      <td>497</td>
      <td>26458</td>
      <td>The Outsider</td>
      <td>The Outsider</td>
      <td>2018</td>
      <td>6.2</td>
      <td>movie</td>
      <td>0</td>
      <td>120</td>
      <td>Action, Crime, Drama</td>
    </tr>
    <tr>
      <th>484</th>
      <td>tt8851668</td>
      <td>498</td>
      <td>26372</td>
      <td>The Lovebirds</td>
      <td>The Lovebirds</td>
      <td>2020</td>
      <td>6.1</td>
      <td>movie</td>
      <td>0</td>
      <td>86</td>
      <td>Action, Comedy, Crime</td>
    </tr>
    <tr>
      <th>494</th>
      <td>tt6521876</td>
      <td>499</td>
      <td>26148</td>
      <td>Jay and Silent Bob Reboot</td>
      <td>Jay and Silent Bob Reboot</td>
      <td>2019</td>
      <td>5.6</td>
      <td>movie</td>
      <td>0</td>
      <td>105</td>
      <td>Action, Adventure, Comedy</td>
    </tr>
  </tbody>
</table>
<p>496 rows × 11 columns</p>
</div>



# Exploring enriched dataset for missing values:


```python
df.isnull().sum()
```




    movie_id         0
    rank             0
    votes            0
    title            0
    originalTitle    0
    year             0
    rating           0
    titleType        0
    isAdult          0
    runtime          0
    genres           0
    dtype: int64



The merged dataset 'df' has **496** rows and **11** columns. **4 observations** were removed from the dataset due to a mismatch in the movies between the two original datasets. Apart from this, there are no missing values in the merged dataset. 

# Export the enriched dataset to a CSV file:


```python
# Use the following naming convention: 
#  Project_3_PartA_Group#.csv

df.to_csv('Project_3_PartA_Patki_Soham.csv')

```
