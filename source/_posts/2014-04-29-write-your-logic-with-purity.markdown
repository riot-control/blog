---
layout: post
author: donbonifacio
title: "Write your logic with purity"
comments: true
categories:
- software development
---

When I first learned about [pure functions](http://www.riot-control.net/2014/04/10/pure-functions/)
I tried to map the concept to a core piece of logic on our product. It was hard,
and I spent some days figuring it out. The test functionality is an invoice
finalizer. We first had all that logic on the fat models, and then we refactored
it to [interactors](/2013/12/03/the-almighty-interactor/)
and it was a huge improvement. Even so, all the _gateway_ logic and such, seamed
like an unnecessary burden. This functionality, in ruby, is something like:

``` ruby
class FinalizeDocument

  attr_accessor :document

  def run
    lock do
      sequence = find_sequence
      document.number = increment(sequence.last_document)
      validate_document

      document.status = 'final'
      document.final_date = Time.now

      document.calculate_totals
      document.sign_and_do_crypto_things

      document.save!
    end
  end

end
```

<!-- more -->

This is all coupled with persistence. It loads models from a gateway and it
saves them to the gateway. But having the gateway concept we can manage to test
this in memory. But do we need the gateway to test this logic? How can we have
this without all that extra noise?

A _pure function_ doesn't mutate objects, and doesn't do IO. Our current version
relies heavily on mutating and IO. Using a pure function feels like improving
the code, but doesn't seam possible on this scenario. I had to reset my way
of thinking to reach something that at this moment seams logical.

The turning point was when I started to say things differently. I don't want
a function that finalizes an invoice. I want a function that when given a
draft invoice, returns a version of that invoice as finalized. On scala, it
could be something like this:

``` scala
case class Document(number : Int, status : String, ...)
case class DocumentItem(unitPrice : Double, quantity : Int, ...)

def finalize(draft:Document, previous:Option[Document]) = {
  val v1 = buildSequence(draft, previous)
  val v2 = calculateTotals(v1)
  signData(v2)
}
```

Using functional programming in scala, we could split all this and build the
`finalize` function as a compositions of the other inner functions. All these
inner functions are also pure an can be unit tested on the side. This is a very
nice way to have indepent, easilly tested logic.

However, we have the logic, but still don't have the functionality. We aren't
persisting anything, so this function doesn't provide us with the side effects
that we want. The thing is, a program can't be solely compoesed of pure functions,
or else it wouldn't do anything usefull. We can however, use our simple and
easilly tested pure function that finalizes a document to implement this
functionality.

``` scala
def finalizeInvoice(draftId) {
  lock {
    val draft = loadDraft(draftId)
    val previous = loadPrevious

    val finalized = finalize(draft, previous)

    saveDocument(finalized)
  }
}
```

This is a better way. We can have all the logic tested and on a module that
doesn't depend on ORMs, web frameworks, etc. And we can have another module
that puts everything together. I believe that this is the best approach for
building software and it's great for TDD.
