# Information Gathering

### Obtain ip
```ping "website.com"```
```nslookup "website.com"```
```whois "website.com"```

### IP / Website Scan
```whatweb "website.com"```
```whatweb "website.com" -v```
```whatweb ip --agression 1-3 -v --no-error```
```whatweb ip --agression 1-3 -v --no-error --log-verbose=result```

### Gathering Emails using theHarvester & Hunter.io
```theHarvester -d "ip" -b all```

### Sherlock Finding username.
```python3 sherlock.py username```