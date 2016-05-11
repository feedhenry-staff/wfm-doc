# RHMAP 3.10.0 Release notes for WFM

## Known issues

### Cloud app
* After creating an WFM project from the project template, the appform ids in the the `lib/app/workflow.js` folder must be updated with new appform ids.

### Portal client
* WFM-portal works in Chrome only
  ([RHMAP-5388](https://issues.jboss.org/browse/RHMAP-5388))
* Scheduler
  * Unscheduled workorder list is depopulated on selected date change
    ([RHMAP-6260](https://issues.jboss.org/browse/RHMAP-6260))
  * Only the left-hand-side of a scheduled workorder is draggable
    ([RHMAP-5910](https://issues.jboss.org/browse/RHMAP-5910))
* Workers
  * Cancel button doesn't work when editing workers
    ([RHMAP-6111](https://issues.jboss.org/browse/RHMAP-6111))
  * Editing a worker after deleting a group he is in is not possible
    ([RHMAP-6126](https://issues.jboss.org/browse/RHMAP-6126))
* Groups
  * When group is not specified, it is not possible to edit a worker
    ([RHMAP-6396](https://issues.jboss.org/browse/RHMAP-6396))
  * First three groups have hardcoded workers
    ([RHMAP-6130](https://issues.jboss.org/browse/RHMAP-6130))
* Messages
  * Search on the messages page doesn't work
    ([RHMAP-6097](https://issues.jboss.org/browse/RHMAP-6097))
* Workflows
  * Search doesn't work on the Workflows page
    ([RHMAP-6135](https://issues.jboss.org/browse/RHMAP-6135))
  * Re-ordering of steps is broken
    ([RHMAP-6136](https://issues.jboss.org/browse/RHMAP-6136))
  * Adding steps is borken; results in corrupted data
    ([RHMAP-6139](https://issues.jboss.org/browse/RHMAP-6139))
  * Workflows should not be deleted if they are used by a workorders
    ([RHMAP-6141](https://issues.jboss.org/browse/RHMAP-6141))
* Forms
  * Search doesn't work on the Forms page
    ([RHMAP-6133](https://issues.jboss.org/browse/RHMAP-6133))
  * Appform preview on portal is missing the iphone background image
    ([RHMAP-6768](https://issues.jboss.org/browse/RHMAP-6768))
* Analytics
  *

### Mobile client
* Offline is currently not supported
* Calendar
  * Shows placeholder workorders
    ([RHMAP-6102](https://issues.jboss.org/browse/RHMAP-6102))
* Workflows
  * Datestamp field doesn't work when configured as date and time
    ([RHMAP-5956](https://issues.jboss.org/browse/RHMAP-5956))
* Files
  * Photo cannot be captured through Photo capture field on device
* Appform integration
([RHMAP-6113](https://issues.jboss.org/browse/RHMAP-6113))
  * Only the *Text*, *Number*, *Location*, *Photo Capture*, *Signature Capture*, and *Datestamp* appform fields are supported
    ([RHMAP-2956](https://issues.jboss.org/browse/RHMAP-2956))
  * Field rules are unsupported
    ([RHMAP-6144](https://issues.jboss.org/browse/RHMAP-6144))
  * Multi-page appforms are unsupported
    ([RHMAP-2224](https://issues.jboss.org/browse/RHMAP-2224))
  * Appform submission upload intermittently fails
    ([RHMAP-5874](https://issues.jboss.org/browse/RHMAP-5874))
  * Datestamp field doesn't work when configured as date and time
    ([RHMAP-5956](https://issues.jboss.org/browse/RHMAP-5956))
* Sync integration
  * Mobile app crashes on connectivity change
    ([RHMAP-](https://issues.jboss.org/browse/RHMAP-6433))
*
  *
    ([RHMAP-](https://issues.jboss.org/browse/RHMAP-))
