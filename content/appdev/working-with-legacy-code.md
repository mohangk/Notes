---
draft: true
title: Working With Legacy Code
categories:
  - Appdev
---
## Working with legacy code

### Legacy management strategy

1. Identify change point
   1. Focus on the easiest one to cover with tests first
2. Find an inflection point
   1. Narrow interface that need to be changes if the underlying classes change
   2. Add tests to the inflection point, when changes are done we can determine if we broke anything by the tests at the inflection point
   3. Move outwards from the places you are going to change the software, look for a narrom interface
3. Conver the inflection point
   1. Break external dependencies
   2. Break internal dependencies
   3. Write tests
4. Make changes
5. Refactor the covered  code
