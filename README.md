# Running Zikipedia on `localhost` *(your workstation)*
* Install the **[Ruby Programming Language](https://www.ruby-lang.org/en/)**

* Install dependencies:
```shell
gem install jekyll
gem install bundler
bundle install .
bundle update .
```

* Start Zikipedia on `localhost`.
```shell
bundle exec jekyll serve
```

&nbsp;

&nbsp;

# Contribution Guidelines

&nbsp;

## z/OS Dictionary Contributions:
* Format all definitions as follows:
```markdown
### ACRONYM/Term *(Expanded Acronym if necessary)*
> 💡 _Try to tie the ACRONYM/Term to something someone new to z/OS already knows to help demystify the concept._

* Give a clear and concise definition. Use terminology and concepts that someone who it new already might understand.
```
* The dictionary must be kept alphabetized.
* The dictionary markdown files are maintained in the **[`dictionary`](dictionary)** folder.

## Contributing to Guides

* How-to guides and step-by-step instructions should be contributed as guides.

* The Guides markdown files are maintained in the **[`guides`](guides)** folder.

* New guides must include a metadata section at the top of the markdown file that defines the markdown file as a guide.
```markdown
---
layout: default
parent: Guides
---
```

## Contributing to References

* Quick references for commands and code syntax should be contributed as references.

* The References markdown files are maintained in the **[references](references)** folder.

* New referencs should include a metadata section at the top of the markdown file that defines the markdown file as a reference.
```markdown
---
layout: default
parent: References
---
```

## Pull Request
* When Contributions are ready, they must be made using a **[pull request](../../pulls)**.