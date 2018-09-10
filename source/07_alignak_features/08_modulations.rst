.. _alignak_features/modulations:

=======================
Time period modulations
=======================

Sometimes you will need to have a different behavior for the Alignak framework depending upon a specific time frame. This is where the modulations are of interest to you.

You can adapt the checks, macros or business impact during a specific time period.

.. _alignak_features/macros_modulations:

Macro modulations
-----------------

It's a good idea to have macros for critical/warning levels on the host or its templates. But sometime even with this, it can be hard to manage such cases where you want to have high levels during the night, and lower levels one during the day.

macro_modulations is made for this.


::

    define  macromodulation{
        macromodulation_name            HighDuringNight
        modulation_period               night
        _CRITICAL                       20
        _WARNING                        10
    }


    define host{
        check_command                   check_ping
        check_period                    24x7
        host_name                       localhost
        use                             generic-host
        macromodulations                HighDuringNight
        _CRITICAL                       5
        _WARNING                        2
    }

With this definition, the values of the _CRITICAL and _WARNING macros will be set to 5 and 2 during the day, and will automatically be set to 20 and 10 during the night timeperiod. You can have as many modulations as you want.

.. note:: if some macros modulations overlap, the first modulation enabled will take the lead.


.. _alignak_features/businessimpact_modulations:

Businessimpact modulations
--------------------------

Depending on your configuration you may want to change the business impact of a specific host/service during the night. For example you don't consider a specific application as business critical during the night because there are no users, so the impact may be lower for this time frame.


::

    define businessimpactmodulation{
        business_impact_modulation_name LowImpactOnNight
        business_impact                 1
        modulation_period               night
    }

    define service{
        check_command                   check_crm_status
        check_period                    24x7
        host_name                       CRM
        service_description             CRM_WEB_STATUS
        use                             generic-service
        business_impact                 3
        businessimpactmodulations       LowImpactOnNight
    }

With this configuration the business impact of the service will be set to 1 during night wheres it is usually 3.



.. _alignak_features/checks_modulations:

Check modulations
-----------------

Depending on your configuration you may want to change the check_command during the night. As an example, you want to send more packets for a ping during the night because there is less network activity so that you can get more accurate data.


::

    define checkmodulation{
        checkmodulation_name            ping_night
        check_command                   check_ping_night
        check_period                    night
    }

    define host{
        check_command                   check_ping
        check_period                    24x7
        host_name                       localhost
        use                             generic-host
        checkmodulations                ping_night
    }

With this configuration, the check_ping command will be replaced with check_ping_night for the host localhost.