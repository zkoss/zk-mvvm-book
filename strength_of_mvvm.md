# Strength of MVVM

**Separation of data and logic from presentation**

The key feature is that ViewModel knows nothing about View's visual elements guarantees the one way dependency from View to the ViewModel thus avoiding mutual programming ripple effects between UI and the ViewModel. Consequently, it brings the following advantages:

* It's suitable for **design-by-contract** programming.<sub>[4]</sub> As long as the contract is made (what data to show and what actions to perform), the UI design and coding of ViewModel can proceed in parallel and independently. Either side will not block the other's way.
* **Loose coupling with View.** UI design can be easily changed from time to time without modifying the ViewModel as long as the contract does not change.
* **Better reusability.** It will be easier to design different views for different devices with a common ViewModel. For a desktop browser with a bigger screen, more information can be shown on one page; while for a smart phone with limited display space, designing a wizard-based step-by-step operation UI can be done without the need to change (much of) the ViewModel.
* **Better testability.** Since ViewModel does not "see" the presentation layer, developers can unit-test the ViewModel class easily without UI elements.

[4]: Design by contract
http://en.wikipedia.org/wiki/Design_by_contract
