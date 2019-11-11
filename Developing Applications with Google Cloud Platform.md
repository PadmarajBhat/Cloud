##### GCP Core Infrastructure:
* Resource Hierarchy:
  * Org -> Folder -> Resources
  * Policies at higher level are inherited to lower level.
  * Hence there is need to keep the generic/low restricted policies at top
  * Policies at lower level overrides the policies on higher one. So whats the use of top level policies ? It just provides the initial policies, creator at that level defines the concious policies.
  * Org node is mandatory only when folder is required.
  * Service account provide policies to resource instances. They use keys to authorize the resource usage. Other polices are to the roles and password driven.
  * Primitive roles affect all resources in a GCP project. Predefined roles apply to a particular service in a project. A answer that took 8 attempts :P
