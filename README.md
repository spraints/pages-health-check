# GitHub Pages Health Check

*Checks your GitHub Pages site for common DNS configuration issues*

[![Build Status](https://travis-ci.org/github/pages-health-check.svg)](https://travis-ci.org/github/pages-health-check) [![Gem Version](https://badge.fury.io/rb/github-pages-health-check.svg)](http://badge.fury.io/rb/github-pages-health-check)

## Installation

`gem install github-pages-health-check`

## Usage

### Basic Usage

```ruby
> check = GitHubPages::HealthCheck.new("choosealicense.com")
=> #<GitHubPages::HealthCheck @domain="choosealicense.com" valid?=true>
> check.valid?
=> true
```

### An invalid domain

```ruby
> check = GitHubPages::HealthCheck.new("foo.github.com")
> check.valid?
=> false
> check.valid!
=> GitHubPages::HealthCheck::InvalidCNAME
```


### Retrieving specific checks

``` ruby
> check.should_be_a_record?
=> true
> check.a_record?
=> true
```

### Getting checks in bulk

```ruby
> check.to_hash
=> {
 :cloudflare_ip?=>false,
 :old_ip_address?=>false,
 :a_record?=>true,
 :cname_record?=>false,
 :valid_domain?=>true,
 :apex_domain?=>true,
 :should_be_a_record?=>true,
 :pointed_to_github_user_domain?=>false,
 :pointed_to_github_pages_ip?=>false,
 :pages_domain?=>false,
 :valid?=>true
}
> require "json"
> check.to_json
=> "{\"cloudflare_ip?\":false,\"old_ip_address?\":false,\"a_record?\":true,\"cname_record?\":false,\"valid_domain?\":true,\"apex_domain?\":true,\"should_be_a_record?\":true,\"pointed_to_github_user_domain?\":false,\"pointed_to_github_pages_ip?\":false,\"pages_domain?\":false,\"valid?\":true}"
```

### Getting the reason a domain is invalid

```ruby
> check = GitHubPages::HealthCheck.new "developer.facebook.com"
> check.valid?
=> false
> check.reason
=> #<GitHubPages::HealthCheck::InvalidCNAME>
> check.reason.message
=> "CNAME does not point to GitHub Pages"
```
