# aflyen.github.io

## Writing

### Code markup

**Snippets:**

```
{% highlight powershell %}
Write-Host "Hello!"
{% endhighlight %}
```

**Onelines:**
```
`Write-Host Hello!`
```

**Gists:**
```
```

## Development environment

1. Install Jekyll: https://jekyllrb.com/docs/
2. Setup: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll
3. Run
`
bundle exec jekyll serve
`
4. If this fails with 'cannot load such file -- webrick', see fix: https://talk.jekyllrb.com/t/upgrade-to-ruby-3-tutorial/6113/3

## Migration from Wordpress (wordpress.com)

This blog was originally hosted on wordpress.com from 2012 until 2022.

1. Download content (XML) from wordpress.com using "Export all"
2. Install jekyll-import: https://import.jekyllrb.com/docs/installation/
3. Set up plugin and rundt import: https://import.jekyllrb.com/docs/wordpressdotcom/