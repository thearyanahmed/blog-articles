Recently, I had a simple task to perform, I had a list of categories, that needs to be sorted alphabetically.

Simple enough, it was a laravel project and I had an array of categories. So, simply, what I did is
```php

collect($categories)->sortBy('title');
```

Simple enough and it was working. One of the fact was we had categories in 2 locales. Greek and English. It was okay with english, and greek was working too. Except there was one exception.

My list was suppose to be

```

Ανυψούμενοι Πάγκοι Εργασίας - Μονταρίσματος
Βενζινοκίνητα
....
Όργανα Μέτρησης
```

but with the code mentioned above, what I got is,

```

Όργανα Μέτρησης
Ανυψούμενοι Πάγκοι Εργασίας - Μονταρίσματος
Βενζινοκίνητα
```
Όργανα was not supposed to be on top. So, I tried many things. Manually sorting, even the first answer were not giving me the desired result.

Then I came to know about PHP Collator class. What is this class for ? php.net says

> Provides string comparison capability with support for appropriate locale-sensitive sort orderings.

So, looks like the amazing people at PHP has something for me. And I tried the initial way, (new Collator)->sort() , didn't work out either.

So back to the drawing board. But while searching this, it came to mind that we if we can sort unicodes, that should do the trick.

And guess what? PHP Collator has a compare function that compares two

```
// Compare two Unicode stringspublicfunctioncompare($str1, $str2) { }
```

And I can use it pass it through a simple sorting function. So my code was finally,

```
usort($categories,function ($a,$b)use ($col){
	return $col->compare($a['title'],$b['title']);
});
```

And finally that did the trick. So, next time if you are sorting some string data and want it to be safe, handle these edge cases like accents and stuff, PHP Collator would be the class to use.