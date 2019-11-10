##### GCP Core Infrastructure:
* Resource Hierarchy:
  * Org -> Folder -> Resources
  * Policies at higher level are inherited to lower level.
  * Hence there is need to keep the generic/low restricted policies at top
  * Policies at lower level overrides the policies on higher one. So whats the use of top level policies ? It just provides the initial policies, creator at that level defines the concious policies.
