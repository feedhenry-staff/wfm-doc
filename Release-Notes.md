# RHMAP 3.10.0 Release notes for WFM

## Known issues

### Cloud app
* After creating an WFM project from the project template, the appform ids in the the `lib/app/workflow.js` folder must be updated with new appform ids.

### Portal client
* WFM-portal works in Chrome only
* Scheduler
  * Unscheduled workorder list is depopulated on selected date change
  * Only the left-hand-side of a scheduled workorder is draggable
* Workers
  * Cancel button doesn't work when editing workers
  * Editing a worker after deleting a group he is in is not possible
* Groups
  * When group is not specified, it is not possible to edit a worker
  * First three groups have hardcoded workers
* Messages
  * Search on the messages page doesn't work
* Workflows
  * Search doesn't work on the Workflows page
  * Re-ordering of steps is broken
  * Adding steps is borken; results in corrupted data
  * Workflows should not be deleted if they are used by a workorders
* Forms
  * Search doesn't work on the Forms page
  * Appform preview on portal is missing the iphone background image
* Analytics
  *

### Mobile client
* Offline is currently not supported
* Calendar
  * Shows placeholder workorders
* Workflows
  * Datestamp field doesn't work when configured as date and time
* Files
  * Photo cannot be captured through Photo capture field on device
* Appform integration
  * Only the *Text*, *Number*, *Location*, *Photo Capture*, *Signature Capture*, and *Datestamp* appform fields are supported
  * Field rules are unsupported
  * Multi-page appforms are unsupported
  * Appform submission upload intermittently fails
  * Datestamp field doesn't work when configured as date and time
* Sync integration
  * Mobile app crashes on connectivity change
