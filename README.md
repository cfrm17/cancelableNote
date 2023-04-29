# Cancellable Note Valuation

During a lifetime of a structured product events will happen that affect its value. Generally, these can be grouped as follows:

Payment events: On these, payments are made to one of the parties in the deal. Market observables that the payment depends on are fixed on a fixing date, and the amount to be paid is accrued over the accrual period, specified by start and end dates. In practice the dates are rolled according to rolling conventions and holiday calendars used, so one refers to adjusted and unadjusted dates 1 . The significance is that, intuitively, a payment is made for the period between unadjusted start date and unadjusted end date.

Cancellation events: On these, the remaining part of the trade is potentially cancelled as a result of a trigger condition or an exercise option. Commonly there is a penalty or redemption payment associated. The decision to exercise is made, or the trigger condition is checked on notice date, and the cancellation takes effect on the effective cancellation date. What this means in practice will depend on the product definition. In most cases, once a cancellation occurred, no payments are made after the effective cancellation date. Similarly to a fixing date, notice date is always adjusted, and similarly to start and end date, one can talk about adjusted or unadjusted effective dates.

Practitioners commonly think (and describe products) in terms of accrual periods, so that each payment is made for a period and a cancellation event cancels all periods starting from the next one. In contrast, generic products normally only refer to one date per event, (indeed, syntactically an event in a generic product definition is associated with a single date). In practice, the date chosen is usually the first date at which all the information is available to calculate the payment value or make a decision to cancel. Thus fixing date is normally the event date for payment events and notice date is normally the event date for cancellation events.


This means that in practice, in generic product description it is not unusual to end up with a product definition where the effect of cancellation is to cancel a payment that was already calculated (when fixing date for the cancellable leg happens to be before the notice date), or a payment after, but not immediately after the notice date event (this situation is quite common with products where a cancellable leg is fixed in arrears).

Legs in a generic product are represented by columns of related events, and generally correspond to product legs as described in a term-sheet. The values of paying legs are total values of all payments on those legs (regardless of cancellation) and the value of a cancellation leg is the negated total value of cancelled flows on the paying legs.

This is a rather broad definition, covering both trigger-type products and callable products. In practice even for callable products the decision to exercise will depend on the current state of the market, and so these are often modeled by introducing some kind of exercise boundary 6 , i.e. a function of market observables describing a multidimensional boundary beyond which it is optimal to exercise. This has the advantage of separating two problems - making a decision to exercise and calculating the value of the cancellation leg. 

For the most part, the subject of this document is the later of the two, that is, the correct calculation of the value of a cancellation leg. There are special cases where the two can be merged to great advantage that cannot be ignored and these are considered below. We will, except where otherwise stated, assume existence of a predicate Cancellation Condition indicating, when computed for a particular cancellation event, whether cancellation occurred.

In general, there may be any number of cancellation legs in a product, and a cancellation leg will cancel a fixed number of other legs. Legs that can be cancelled as an effect of valuation of a cancellation leg will be referred to as cancellable legs. It is possible for a cancellation leg to be cancellable by another cancellation leg. We will however assume that in cases where there are more than one cancellation legs cancelling same legs the term-sheet defines clearly an order of precedence between them (i.e. which decision is made first), and that if two cancellation legs cancel a single leg in common, then their actions are mutually exclusive (i.e. cancellation can only occur on one of them).

These assumptions are not restrictive in practice as they will hold for any “reasonable” product, and even if they do not, it is always possible to split a single cancellation leg into a number of legs, each checking the same condition on same dates but affecting different cancellable legs. We will also assume that all legs cancellable by a particular cancellation legs appear to the left of it in the product definition. This makes computation more efficient and helps prevent errors in coding, and is again not restrictive as all Felix valuation engines consider product legs in isolation.

Further, we will be concerned only with Bermudan trades, that is, we will assume that the decision to cancel the deal must be made on one of the set of pre-agreed dates. This type of products fit naturally within the structure of generic products definition language, and we are still to come across a realistic product that cannot be described in terms of a finite set of dates.

Finally, depending on whether the valuation environment is backwards looking (Monte Carlo) or forward looking (backwards induction engines), value of a cancellation leg is calculated differently and the two cases will be considered separately.


Reference:

https://finpricing.com/FinPricing-ProductBrochure.pdf
