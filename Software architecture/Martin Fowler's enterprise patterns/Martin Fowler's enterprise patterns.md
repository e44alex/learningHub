
https://www.martinfowler.com/eaaCatalog/index.html

1) **Domain Logic Patterns:**
	1) [Transaction Script](https://www.martinfowler.com/eaaCatalog/transactionScript.html) (110), 
	2) [Domain Model](https://www.martinfowler.com/eaaCatalog/domainModel.html) (116),
	3) [Table Module](https://www.martinfowler.com/eaaCatalog/tableModule.html) (125),
	4) [Service Layer](https://www.martinfowler.com/eaaCatalog/serviceLayer.html) (133).
2) **Data Source Architectural Patterns:**
	1) [Table Data Gateway](https://www.martinfowler.com/eaaCatalog/tableDataGateway.html) (144),
	2) [Row Data Gateway](https://www.martinfowler.com/eaaCatalog/rowDataGateway.html) (152),
	3) [Active Record](https://www.martinfowler.com/eaaCatalog/activeRecord.html) (160),
	4) [Data Mapper](https://www.martinfowler.com/eaaCatalog/dataMapper.html) (165).

3) **Object-Relational Behavioral Patterns:**
	1) [x] [Unit of Work](https://www.martinfowler.com/eaaCatalog/unitOfWork.html) (184), [[Unit of work]]
	2) [x] [Identity Map](https://www.martinfowler.com/eaaCatalog/identityMap.html) (195)  Also mentioned in [[Unit of work]]
	3) [x] [Lazy Load](https://www.martinfowler.com/eaaCatalog/lazyLoad.html) (200) [[Lazy loading]]

4) **Object-Relational Structural Patterns:**
	1) [Identity Field](https://www.martinfowler.com/eaaCatalog/identityField.html) (216), 
	2) [Foreign Key Mapping](https://www.martinfowler.com/eaaCatalog/foreignKeyMapping.html) (236), 
	3) [Association Table Mapping](https://www.martinfowler.com/eaaCatalog/associationTableMapping.html) (248),
	4) [Dependent Mapping](https://www.martinfowler.com/eaaCatalog/dependentMapping.html) (262), 
	5) [Embedded Value](https://www.martinfowler.com/eaaCatalog/embeddedValue.html) (268),
	6) [Serialized LOB](https://www.martinfowler.com/eaaCatalog/serializedLOB.html) (272), 
	7) [Single Table Inheritance](https://www.martinfowler.com/eaaCatalog/singleTableInheritance.html) (278), 
	8) [Class Table Inheritance](https://www.martinfowler.com/eaaCatalog/classTableInheritance.html) (285),
	9) [Concrete Table Inheritance](https://www.martinfowler.com/eaaCatalog/concreteTableInheritance.html) (293),
	10) [Inheritance Mappers](https://www.martinfowler.com/eaaCatalog/inheritanceMappers.html) (302).

5) **Object-Relational Metadata Mapping Patterns:** 
	1) [Metadata Mapping](https://www.martinfowler.com/eaaCatalog/metadataMapping.html) (306), 
	2) [Query Object](https://www.martinfowler.com/eaaCatalog/queryObject.html) (316), 
	3) [x] [Repository](https://www.martinfowler.com/eaaCatalog/repository.html) (322).

6) **Web Presentation Patterns:** 
	1) [x] [Model View Controller](https://www.martinfowler.com/eaaCatalog/modelViewController.html) (330), [[MVC]]
	2) [x] [Page Controller](https://www.martinfowler.com/eaaCatalog/pageController.html) (333),
	3) [Front Controller](https://www.martinfowler.com/eaaCatalog/frontController.html) (344), 
	4) [x] [Template View](https://www.martinfowler.com/eaaCatalog/templateView.html) (350), // used in Razor alike templates, now everywhere in FE
	5) [Transform View](https://www.martinfowler.com/eaaCatalog/transformView.html) (361), 
	6) [Two-Step View](https://www.martinfowler.com/eaaCatalog/twoStepView.html) (365),
	7) [Application Controller](https://www.martinfowler.com/eaaCatalog/applicationController.html) (379).

7) **Distribution Patterns:**
	1) [ ] [Remote Facade](https://www.martinfowler.com/eaaCatalog/remoteFacade.html) (388),
	2) [x] [Data Transfer Object](https://www.martinfowler.com/eaaCatalog/dataTransferObject.html) (401) [[DTO]]

8) **Offline Concurrency Patterns:**
	1) [Optimistic Offline Lock](https://www.martinfowler.com/eaaCatalog/optimisticOfflineLock.html) (416),
	2) [Pessimistic Offline Lock](https://www.martinfowler.com/eaaCatalog/pessimisticOfflineLock.html) (426),
	3) [Coarse Grained Lock](https://www.martinfowler.com/eaaCatalog/coarseGrainedLock.html) (438),
	4) [Implicit Lock](https://www.martinfowler.com/eaaCatalog/implicitLock.html) (449).

9) **Session State Patterns:**
	1) [ ] [Client Session State](https://www.martinfowler.com/eaaCatalog/clientSessionState.html) (456),
	2) [ ] [Server Session State](https://www.martinfowler.com/eaaCatalog/serverSessionState.html) (458),
	3) [ ] [Database Session State](https://www.martinfowler.com/eaaCatalog/databaseSessionState.html) (462).

10) **Base Patterns:**
	1) [x] [Gateway](https://www.martinfowler.com/eaaCatalog/gateway.html) (466), [[Gateway]]
	2) [ ]  [Mapper](https://www.martinfowler.com/eaaCatalog/mapper.html) (473),
	3) [ ]  [Layer Supertype](https://www.martinfowler.com/eaaCatalog/layerSupertype.html) (475),
	4) [ ]  [Separated Interface](https://www.martinfowler.com/eaaCatalog/separatedInterface.html) (476),
	5) [ ]  [Registry](https://www.martinfowler.com/eaaCatalog/registry.html) (480), 
	6) [ ]  [Value Object](https://www.martinfowler.com/eaaCatalog/valueObject.html) (486),
	7) [ ]  [Money](https://www.martinfowler.com/eaaCatalog/money.html) (488), 
	8) [ ]  [Special Case](https://www.martinfowler.com/eaaCatalog/specialCase.html) (496),
	9) [ ]  [Plugin](https://www.martinfowler.com/eaaCatalog/plugin.html) (499),
	10) [ ]  [Service Stub](https://www.martinfowler.com/eaaCatalog/serviceStub.html) (504),
	11) [ ]  [Record Set](https://www.martinfowler.com/eaaCatalog/recordSet.html) (508)


Specification: https://codewithmukesh.com/blog/specification-pattern-in-aspnet-core/ [[Specification]]
