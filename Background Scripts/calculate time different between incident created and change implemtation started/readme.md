***Calculate the time difference between Incident cretead and change implementation started.***

Assume that a incident got created and later fulfiller team feels that this needs to get moved as change request, so incident will get promoted to change request, so by using this code if we run a schedule job on weekly basics the manager or lead will get know the fullfillment time teams are mainitaing to complete the end user requests.


**Code explanation**

**Part 1**
  // Query high-impact incidents
  var incidentGR = new GlideRecord('incident');
  incidentGR.addQuery('impact', '1'); 
  incidentGR.query();

**Part 2**
    // Find related change requests
  while (incidentGR.next()) {
    var changeGR = new GlideQuery('change_request');
    changeGR.addQuery('incident', incidentGR.getUniqueValue());
    changeGR.query();

 **Part 3**
      // Calculate the time difference between incident creation and change implementation
    if (changeGR.next()) {
      var createdTime = new GlideDateTime(incidentGR.sys_created_on);
      var implementationTime = new GlideDateTime(changeGR.scheduled_start);
      var timeDifference = createdTime.subtract(implementationTime).getNumericValue() / 3600; // in hours

  **Part 4**
    // Update a custom field in the incident table with the time difference
      incidentGR.u_time_to_change_implementation = timeDifference;
      incidentGR.update();
    }
  }

 
