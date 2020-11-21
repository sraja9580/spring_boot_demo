Spring
	* Application Framework (DI)
	* Programing and Configuration Model (lets you write business logic)
	
	Problems with Spring
		
		* Huge framework
		* Multiple Setup steps
		* Multiple Configuration Steps
		* Multiple Build and Deploy Steps
	
Spring Boot
	Spring boot makes it easy to create stand-alone, production grade spring based application that you can just run
	
	* Opinionated
	* Convention over configuration
	* Stand Alone (with spring we create war or ear which needs to app server)
	* production ready
	
Bill of Materials 
	Works with starter parent
	BOM stands for Bill Of Materials. 
	A BOM is a special kind of POM that is used to control the versions of a project’s dependencies and provide a central place to define and update those versions.
	BOM provides the flexibility to add a dependency to our module without worrying about the version that we should depend on.

Why Embedded Tomcat Server
	Convenience
	Servlet Container configuration is part of application(application.props)
	Standalone application(Easy to develop,run,deploy)
	
Spring MVC in Boot project?
	added to project -> with web starter
		if the incoming url and http method is matched with any of the controller class then executes the method
		convert the response to appropriate formate and sends(we return pojos boot converts into specific format)
		
	

	
