# Extensions on Iterable to check if all distinct or all Equal

* **Type**: Standard Library API proposal
* **Author**: Daniil Vodopian
* **Contributors**: Sergey Igushkin
* **Status**: Submitted
* **Prototype**: Not started
* **Related issues**: [KT-10380](https://youtrack.jetbrains.com/issue/KT-10380), [KT-30270](https://youtrack.jetbrains.com/issue/KT-30270)



## Summary

Add functionality to check when all elements of an Iterable or a collection 
are similar to each other and when all elements are distinct.

## Similar API review

* Is there a similar functionality in the standard library?
* How the same/similar concept is implemented in other languages/frameworks?

## Use cases

* Provide several *real-life* use cases (either links to public repositories or ad-hoc examples).

## Alternatives

* How verbose would be these use cases without the API proposed?

## Dependencies

What are the dependencies of the proposed API:

* a subset of Kotlin Standard Library available on all supported platforms.
* JDK-specific dependencies, specify minimum JDK version.
* JS-specific dependencies.

## Placement

Standard Library in `kotlin.collectrions`
together with [`Iterable<T>.all {...}`](http://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/all.html)

## Reference implementation

    fun <T> Iterable<T>.allEqual(): Boolean
        = allEqualBy { it }
     
    inline fun <T, S> Iterable<T>.allEqualBy(selector: (T) -> S): Boolean {
        when(size) {
            0, 1 -> return true    
        }        
  
        val firstSelected = first().selector()
        return this.all { it.selector() == firstSelected }
    }

    fun <T> Iterable<T>.allDistinct(): Boolean
        = allDistinctBy { it }

    inline fun <T, S> Iterable<T>.allDistinctBy(selector: (T) -> S): Boolean {
        when(size) {
            0, 1 -> return true    
        }  
                
        val mutSet = mutableSetOf<S>()
        return this.all { e -> mutSet.add(selector(e)) }
    }

* Provide the reference implementation and test cases.
In case if the API should be specialized for each primitive, only one reference implementation is enough.
* Provide the answers for the questions from the [Appendix](#appendix-questions-to-consider) in case they are not trivial.

## Unresolved questions

* List unresolved questions if any.
* Provide options to solve them.

## Future advancements

* What are the possible and most likely extension points?


-------

# Appendix: Questions to consider
These questions are not a part of the proposal,
but be prepared to provide the answers if they aren't trivial.


## Additional considerations for collection operations

### Receiver types

Consider if the operation could be provided not only for collections,
but for other collection-like receivers such as:

* arrays
* sequences
* strings and char sequences
* maps
* ranges

It is helpful to determine what are the collection requirements:

* any iterable or sequence
* has size
* has fast indexed access
* has fast `contains` operation
* allows to mutate its elements
* allows to add/remove elements

