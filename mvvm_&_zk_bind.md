# MVVM & ZK Bind

Since the ViewModel contains no reference to UI components, it needs a mechanism to synchronize data between the View and ViewModel. Additionally, this mechanism has to accept the user request from the View layer and bridge the request to the action and logic provided by the ViewModel layer. This mechanism, the kernel part of the MVVM design pattern, is either data synchronising codes written by the application developers themselves or a data binding system provided by the framework. ZK 6 provides a data binding mechanism called **ZK Bind** to be the infrastructure of the MVVM design pattern and the **binder**<sub>[5]</sub> plays the key role to operate the whole mechanism. The binder is like a broker and responsible for communication between View and ViewModel.
![MVVM Architecture](./images/Mvvm-architecture.png)

[5]: binder ZK Developer's [DataBinding/Binder](./data_binding/binder.html)
