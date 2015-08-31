KinomaJS is available as source code through [our repository on GitHub](https://github.com/Kinoma/kinomajs). Our goal in publishing the source code is to allow developers to incorporate KinomaJS into their hardware and software products. The KinomaJS software built by the Kinoma team is provided under the [Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html), which provides a great deal of flexibility in the use of KinomaJS. KinomaJS also incorporates software from other authors, who have selected other licenses for their own software.

The [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html) grants you certain rights. When you use those rights, you agree to comply with the obligations stated in the license. For example, the [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html) grants the right to use the KinomaJS source code without paying a license fee, and you agree to inform your customers that your product incorporates KinomaJS. To be clear, this is just one example from the license. The text of the [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html) is the full official set of rules. The licenses applied by the authors of the other software modules incorporated into KinomaJS have their own set of rules. This FAQ provides a high level overview of how to follow the rules in these licenses.

Before getting into the details, our legal team wants to make sure you are aware that this FAQ is not formal legal advice.

> **Disclaimer**: The content of this FAQ is offered only as a goodwill service to users of KinomaJS, intended for informational purposes only. Nothing in this FAQ shall be considered as or constitute legal advice, and nothing in this FAQ may be used as a substitute for obtaining legal advice from an attorney licensed or authorized to practice in your jurisdiction. No contents herein are intended to create an attorney-client relationship, and no contents herein shall create an attorney-client relationship. There is no guaranty that the information provided here is accurate, complete, or up to date, and Marvell Semiconductor, Inc. accepts no responsibility for any loss which may arise from access or use of the information provided here.

***
##If I use KinomaJS for my own purposes and do not distribute the application, what do I need to do?
If you are using KinomaJS but not distributing it, you may freely use KinomaJS with the rights granted under the [Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html). Because you are not distributing KinomaJS in source code or binary form, there are no users or customers of your product to notify of your use of KinomaJS.

***
##Is there a way to avoid distributing KinomaJS with my product?
If you distribute your product only in source code form, simply provide a link to the KinomaJS source code repository at [https://github.com/Kinoma/kinomajs](https://github.com/Kinoma/kinomajs) instead of including the KinomaJS source code with your source code. Users of your source code can retrieve the KinomaJS source code themselves, so the [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html) redistribution rules do not apply to you, as you are not distributing KinomaJS, only referring users to GitHub.

***
##What do I have to do to distribute KinomaJS?
If you distribute KinomaJS in source code or binary form, you must comply with relevant licenses that define your rights to KinomaJS. Those are located in the [NOTICE](https://github.com/Kinoma/kinomajs/blob/master/NOTICE) document on the [Kinoma GitHub repository](https://github.com/Kinoma/kinomajs).

If you distribute the KinomaJS source code “as is,” with all notices and license information remain intact and easily accessible to the end-user of your product, you are in compliance with the redistribution requirements of the [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html).

If you distribute KinomaJS in executable form (e.g. compiled as part of an application), the [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html) requires you to include a copy of the [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html) itself with the attribution notices contained within the [NOTICE](https://github.com/Kinoma/kinomajs/blob/master/NOTICE), in a human readable form. Information about KinomaJS and notifications that are required by third party open source licenses within KinomaJS has been incorporated into the [NOTICE](https://github.com/Kinoma/kinomajs/blob/master/NOTICE) file. The contents of the [NOTICE](https://github.com/Kinoma/kinomajs/blob/master/NOTICE) file must be easily accessible to the end-user to review using the application you distribute.

***
##How should I display the contents of the NOTICE file to the user of my application?
The [Apache License](http://www.apache.org/licenses/LICENSE-2.0.html) is flexible on the actions the user must take to view the contents of the [NOTICE](https://github.com/Kinoma/kinomajs/blob/master/NOTICE) file. The majority of applications incorporate these files into their About or Settings screens, so consider using this familiar model in your application.

The sample application [license-simple](https://github.com/Kinoma/KPR-examples/tree/master/license-simple) shows one approach for integrating display of the [NOTICE](https://github.com/Kinoma/kinomajs/blob/master/NOTICE) file into a KinomaJS application. This sample is provided as one possible method to present this information. You may develop method another for your application, one which matches your application’s look and feel.

***
##What do I need to do to distribute an application built with Kinoma Studio?
[Kinoma Studio](http://kinoma.com/develop/studio/) is the integrated development environment(IDE) for KinomaJS. When you use Kinoma Studio to package an application for use on iOS, Android, Mac OS X, or Windows the resulting application includes a pre-built copy of the KinomaJS software. That means that any application built for these platforms using Kinoma Studio incorporates a binary of KinomaJS and therefore must comply with the redistribution rules. See the question [“What do I have to do to distribute KinomaJS?”](#-what-do-i-have-to-do-to-distribute-kinomajs) for details on the redistribution rules.

***
##If I include other third party source code in addition to KinomaJS, are there other rules or restrictions on distributing my application?
If the application you build using KinomaJS incorporates other source code (written in any programming language, including C or JavaScript) not written and owned by you, you must confirm that you have permission to use that source code. Use of source code you did not write may carry with it additional obligations which follow from license terms included in that source code or from copyright laws.

***
##Can I add my own distribution rules or restrictions to my application code?
Yes. However, any additional rules cannot modify, limit, or otherwise interfere with the obligations in any third party code, including KinomaJS, incorporated in your application.

***
##Is there another disclaimer?
Yes. The information in this FAQ is only intended to (and can only possibly) give you some assistance in understanding what your obligations might be in distributing applications that incorporate KinomaJS. The rights and responsibilities generated by open source licenses can be complex. If you have questions, you should consult an attorney licensed or authorized to practice in your jurisdiction, who is knowledgeable in this area.
