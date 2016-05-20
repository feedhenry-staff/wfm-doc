# RHMAP 3.10.0 Release notes for WFM

## Known issues

### Cloud app
* After creating an WFM project from the project template, the appform ids in the the `lib/app/workflow.js` folder must be updated with new appform ids.

### Portal client
* WFM-portal works in Chrome only
  (Bug #5388)
* Scheduler
  * Unscheduled workorder list is depopulated on selected date change
    (Bug #6260)
  * Only the left-hand-side of a scheduled workorder is draggable
    (Bug #5910)
* Workers
  * Cancel button doesn't work when editing workers
    (Bug #6111)
  * Editing a worker after deleting a group he is in is not possible
    (Bug #6126)
* Groups
  * When group is not specified, it is not possible to edit a worker
    (Bug #6396)
  * First three groups have hardcoded workers
    (Bug #6130)
* Messages
  * Search on the messages page doesn't work
    (Bug #6097)
* Workflows
  * Search doesn't work on the Workflows page
    (Bug #6135)
  * Re-ordering of steps is broken
    (Bug #6136)
  * Adding steps is borken; results in corrupted data
    (Bug #6139)
  * Workflows should not be deleted if they are used by a workorders
    (Bug #6141)
* Forms
  * Search doesn't work on the Forms page
    (Bug #6133)
  * Appform preview on portal is missing the iphone background image
    (Bug #6768)
* Analytics
  *

### Mobile client
* Offline is currently not supported
* Calendar
  * Shows placeholder workorders
    (Bug #6102)
* Workflows
  * Datestamp field doesn't work when configured as date and time
    (Bug #5956)
* Files
  * Photo cannot be captured through Photo capture field on device
* Appform integration
(Bug #6113)
  * Only the *Text*, *Number*, *Location*, *Photo Capture*, *Signature Capture*, and *Datestamp* appform fields are supported
    (Bug #2956)
  * Field rules are unsupported
    (Bug #6144)
  * Multi-page appforms are unsupported
    (Bug #2224)
  * Appform submission upload intermittently fails
    (Bug #5874)
  * Datestamp field doesn't work when configured as date and time
    (Bug #5956)
* Sync integration
  * Mobile app crashes on connectivity change
    (Bug #6433)
