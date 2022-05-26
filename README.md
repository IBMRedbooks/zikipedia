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

* If you are contributing a new guide, **[`_data/guides.yml`](_data/guides.yml)** should be updated with the link to the new guide. This list should be kept alphabetized.
```yaml
- title: "My New Guide"
  link: /guides/my_new_guide/
```

* New guides should include a metadata section at the top of the markdown file that defines the link to the new guide.
```markdown
---
layout: default
permalink: references/my_new_guide/
---
```

## Contributing to References

* Quick references for commands and code syntax should be contributed as references.

* The References markdown files are maintained in the **[references](references)** folder.

* If you are contributing a new reference, **[`_data/references.yml`](_data/references.yml)** should be updated with the link to the new reference. This list should be kept alphabetized.
```yaml
- title: "My New Reference"
  link: /references/my_new_reference/
```

* New referencs should include a metadata section at the top of the markdown file that defines the link to the new reference.
```markdown
---
layout: default
permalink: references/my_new_reference/
---
```

## Pull Request
* When Contributions are ready, they must be made using a **[pull request](../../pulls)**.