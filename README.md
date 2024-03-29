# Patento

Patento is a simple gem for scraping Google Patents and formatting the visible data into objects and hashes.

## Usage

Patento has two models of operation: local and remote. In local mode, you pass in the .html file saved from Google patents. In remote mode, you simply supply a patent number and Patento will pull the HTML from Google.

Note that pulling remote data will result in one HTTP request per patent requested. Thus, if doing large amounts of downloading, please consider respecting Google's server load. For your convenience, Google's terms of service can be found here: [http://www.google.com/intl/en/policies/terms/](http://www.google.com/intl/en/policies/terms/).

### Examples:
  
    Patent.new(6000000) # Retreives patent details remotely
    
    Patent.new(6000000, local_path: "US6000000.html") # Parses  ./US6000000.html and returns the same data as above

### Data Retreived

Patento retrieves the following data:

1. Patent Number (`String`): `patent.number`
2. Title (`String`): `patent.title`
3. Abstract (`String`): `patent.abstract`
3. Serial (`String` or nil, see **note**): `patent.serial`
  * ***Note***: Patento can only return the serial if it is present at Google (compare patent 6000000 with 7114060). If Google doesn't provide it, serial will be nil
4. Inventors (`Array` of `String` objects): `patent.inventors`
5. Assignee (`String`): `patent.assignee`
6. U.S. Classifications (`Array` of `String` objects): `patent.us_classifications`
7. International Classification (`String`): `patent.intl_classifications`
8. Filing Date (`Date`): `patent.filing_date`
9. Issue Date (`Date`): `patent.issue_date`
  * ***Note***: Patento does not retrieve the publication date of published applications due to this data not being available at Google
10. Claims (`Array` of `Claim` objects (see below)): `patent.claims`
11. Forward Citations (`Array` of `Hash` objects): `patent.forward_citations`
12. Backward Citations (`Array` of `Hash` objects): `patent.backward_citations`

## Citations
Forward and backward citations are just `Hash` objects with the following keys: `number`, `filing`, `issue`, `assignee`, and `title`.

## Claim Objects
Patento stores claim data in separate `Claim` objects. Claim objects have a `number`, a `type` (independent or dependent), a `preamble`, and a `body`. All attributes are public.

All claims are guaranteed to have a `preamble`. Shorter claims (one liners) may *only* have a preamble. 

The `body` element is a nested array of elements corresponding to the indenting of the original claim. For example, the claim:

    Preamble
    Element 1
        Sub-element 1
        Sub-element 2
    Element 2

will have a `body` represented as a nested array like so:

    [
        [
            "Element 1",
            
            [
                "Sub-element 1",
                "Sub-element 2"
            ]
        ],
        
        "Element 2"
    ]

## Contributing to patento
 
* Check out the latest master to make sure the feature hasn't been implemented or the bug hasn't been fixed yet.
* Check out the issue tracker to make sure someone already hasn't requested it and/or contributed it.
* Fork the project.
* Start a feature/bugfix branch.
* Commit and push until you are happy with your contribution.
* Make sure to add tests for it. This is important so I don't break it in a future version unintentionally.
* Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise necessary, that is fine, but please isolate to its own commit so I can cherry-pick around it.

## Copyright

Copyright (c) 2012 George Zalepa. See LICENSE.txt for further details.
