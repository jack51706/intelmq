CHANGELOG
==========

development
-----------
### Development
- We are now testing with and without optional libraries/lowest recommended versions and most current versions of required libraries

### Bots
- HTTP collectors: If http_username and http_password are both given and empty or null, 'None:None' has been used to authenticate. It is now checked that the username evaulates to non-false/null before adding the authentication. (fixes #1017)

#### Parsers
- Removed bots.parsers.openbl as the source is offline since end of may (#1018, https://twitter.com/sshblorg/status/854669263671615489)
- Removed bots.parsers.proxyspy as the source is offline (#1031)
- Shadowserver: Added Accessible SMB
- `bots.experts.ripencc_abuse_contact` now has the two additional parameters `query_ripe_stat_asn` and `query_ripe_stat_ip`.
  Deprecated parameter `query_ripe_stat`. New parameter `mode`.
- `bots.experts.certat_contact` has been renamed to `bots.experts.national_cert_contact_certat` (#995)

v1.0.0.dev8
-----------

### General changes
- It's now configurable how often the bots are logging how much events they have sent, based on both the amount and time. (fixes #743)
- switch from pycodestyle to pep8

### Configuration
- Added `log_processed_messages_count` (500) and `log_processed_messages_seconds` (900) to defaults.conf.
- `http_timeout` has been renamed to `http_timeout_sec` and `http_timeout_max_tries` has been added.
   This setting is honored by bots.collectors.http.* and bots.collectors.mail.collector_mail_url, bots.collectors.rt (only `http_timeout_sec`), bots.outputs.restapi.output and bots.experts.ripencc_abuse_contact

### Documentation
- Minor fixes
- Dropped install scripts, see INSTALL.md for more detailed instructions and explanations
- Better structure of INSTALL.md
- Better documentation of packages

### Tools
- added a bot debugger (https://github.com/certtools/intelmq/pull/975)
- missing bot executable is detected and handled by intelmqctl (https://github.com/certtools/intelmq/pull/979)

### Core
- fix bug which prevented dumps to be written if the file did not exist (https://github.com/certtools/intelmq/pull/986)
- Fix reload of bots regarding logging
- type annotions for all core libraries

### Bots
- added bots.experts.idea, bots.outputs.files
- possibility to split large csv Reports into Chunks, currently possible for mail url and file collector
- elasticsearch output supports HTTP Basic Auth
- bots.collectors.mail.collector_mail_url and bots collectors.file.collector can split large reports (https://github.com/certtools/intelmq/pull/680)
- bots.parsers.shadowserver support the VNC feed
- handling of HTTP timeouts, see above https://github.com/certtools/intelmq/pull/859
- bots.parsers.bambenek saves the malware name
- bots.parsers.fraunhofer.parser_dga saves the malware name
- bots.parsers.shadowserver handles NULL bytes
- bots.parsers.abusech.parser_ransomware handles the IP 0.0.0.0 specially

### Harmonization
- New field named `output` to support export to foreign formats

v1.0.0.dev7
-----------

### Documentation
- more verbose installation and upgrade instructions

### Bot changes
- added bots.experts.field_reducer, bots.outputs.smtp
- bots.collectors.alienvault_otx: OTX library has been removed, install it as package instead
- bots.experts.deduplicator: `ignore_keys` has been renamed to `filter_keys` and `filter_type` has been removed.
- bots.experts.modify: The configration is now list-based for a consistent ordering
- bots.experts.tor_node as an optional parameter `overwrite`
- API keys will be removed from feed.url if possible

### Harmonization
- New parameter and field named feed.documentation to link to documentation of the feed
- classification.taxonomy is lower case only

v1.0.0.dev6
-----------

Changes between 0.9 and 1.0.0.dev6

### General changes
- Dropped support for Python 2, Python >= 3.3 is needed
- Dropped startup.conf and system.conf. Sections in BOTS can be copied directly to runtime.conf now.
- Support two run modes: 'stream' which is the current implementation and a new one 'scheduled' which allows scheduling via cron or systemd.
- Helper classes for parser bots
- moved intelmq/conf to intelmq/etc
- cleanup in code and repository
- All bots capable of reloading on SIGHUP
- packages
- pip wheel format instead of eggs
- unittests for library and bots
- bots/BOTS now contains only generic and specific collectors. For a list of feeds, see docs/Feeds.md

### executables
- DEV: intelmq_gen_harm_docs: added to generate Harmonization documentation
- intelmq_psql_initdb: creates a table for a postgresql database using the harmonization fields
- intelmqctl: reworked argument parsing, many bugfixes
- intelmqdump: added to inspect dumped messages and reinsert them into the queues
- DEV: rewrite_config_files: added to rewrite configuration files with consistent style


### Bot changes
#### Collectors
- added alienvault, alienvault otx, bitsight, blueiv, file, ftp, misp, n6, rtir, xmpp collector
- removed hpfeeds collector
- removed microsoft DCU collector
- renamed and reworked URL collector to HTTP
- reworked Mail collectors

#### Parsers
- source specific parsers added: abusech, alienvault, alienvault otx, anubisnetworks, autoshun, bambenek, bitcash, bitsight, blocklistde, blueliv, ci army, cleanmx, cymru_full_bogons, danger_rulez, dataplane, dshield (asn, block and domain), dyn, fraunhofer_dga, hphosts, malc0de, malwaredomains, misp, n6, netlab_360, nothink, openphish, proxyspy, spamhaus cert, taichung, turris, urlvir
- generic parsers added: csv, json
- specific parsers dropped: abusehelper (broken), arbor (source unavailable), bruteforceblocker, certeu, dragonresearchgroup parser (discontinued), hpfeeds, microsoft_dcu (broken), taichungcitynetflow, torexitnode parser
- renamed intelmq.bots.parsers.spamhaus.parser to intelmq.bots.parsers.spamhaus.parser_drop
  renamed intelmq.bots.parsers.malwarepatrol.parser-dansguardian to intelmq.bots.parsers.malwarepatrol.parser_dansguardian
- renamed intelmq.bots.parsers.taichungcitynetflow.parser to intelmq.bots.parsers.taichung.parser
- major rework of shadowserver parsers
- enhanced all parsers

#### Experts
- Added experts: asnlookup, cert.at contact lookup, filter, generic db lookup, gethostbyname, modify, reverse dns, rfc1918, tor_nodes, url2fqdn
- removed experts: contactdb, countrycodefilter (obsolete), sanitizer (obsolete)
- renamed intelmq.bots.expers.abusix.abusix to bots.expers.abusix.expert
  intelmq.bots.experts.asnlookup.asnlookup to intelmq.bots.experts.asn_lookup.expert
  intelmq.bots.experts.cymru.expert to intelmq.bots.experts.cymru_whois.expert
  intelmq.bots.experts.deduplicator.deduplicator to intelmq.bots.experts.deduplicator.expert
  intelmq.bots.experts.geoip.geopip to intelmq.bots.experts.maxmind_geoip.expert
  intelmq.bots.experts.ripencc.ripencc to intelmq.bots.experts.ripencc_abuse_contact.expert
  intelmq.bots.experts.taxonomy.taxonomy to intelmq.bots.experts.taxonomy.expert
- enhanced all experts
- changed configuration syntax for bots.experts.modify to a more simple variant

#### Outputs
- added: amqp, elasticsearch, redis, restapi, smtp, stomp, tcp, udp, xmpp
- removed: debug, intelmqmailer (broken), logcollector
- enhanced all outputs

### Bug fixes
- FIX: all bots handle message which are None
- FIX: various encoding issues resolved in core and bots
- FIX: time.observation is generated in collectors, not in parsers

### Other enhancements and changes
- TST: testing framework for core and tests. Newly introduced components should always come with proper unit tests.
- ENH: intelmqctl has shortcut parameters and can clear queues
- STY: code obeys PEP8, new code should always be properly formatted
- DOC: Updated user and dev guide
- Removed Message.contains, Message.update methods Message.add ignore parameter

### Configuration
- ENH: New parameter and field named accuracy to represent the accuracy of each feed
- Consistent naming "overwrite" to switch overwriting capabilities of bots (as opposed to override)
- Renamed `http_ssl_proxy` to `https_proxy`
- parameter `hierarchical_output` for many output bots
- deduplicator bot has a new required parameter to configure deduplication mode `filter_type`
- deduplicator bot key ignore_keys was renamed to filter_keys
- The tor_nodes expert has a new parameter `overwrite`, which is by default `false`.

### Harmonization
- ENH: Additional data types: integer, float and Boolean
- ENH: Added descriptions and matching types to all fields
- DOC: harmonization documentation has same fields as configuration, docs are generated from configuration
- BUG: FQDNs are only allowed in IDN representation
- ENH: Removed UUID Type (duplicate of String)
- ENH: New type LowercaseString and UppercaseString, doing automatic conversion
- ENH: Removed UUID Type (duplicate of String)
- ENH: FQDNs are converted to lowercase
- ENH: regex, iregex and length checks when data is added to messages

#### Most important changes:
- `(source|destination).bgp_prefix` is now `(source|destination).network`
- `(source|destination).cc` is now `(source|destination).geolocation.cc`
- `(source|destination).reverse_domain_name` is `(source|destination).reverse_dns`
- `(source|destination).abuse_contact` is lower case only
- `misp_id` changed to `misp.event_uuid`
- `protocol.transport` added, a fixed list of values is allowed
- `protocol.application` is lower case only
- `webshot_url` is now `screenshot_url`
- `additional_information` renamed to `extra`, must be JSON
- `os.name`, `os.version`, `user_agent` removed in favor of `extra`
- all hashes are lower case only
- added `malware.hash.(md5|sha1|sha256)`, removed `malware.hash`
- New parameter and field named feed.accuracy to represent the accuracy of each feed
- New parameter and field named feed.provider to document the name of the source of each feed
- New field `classification.identifier`
-`classification.taxonomy` is now lower case only

### Known issues
 - Harmonization: hashes are not normalized and classified, see also issue #394 and pull #634

### Contrib
- ansible and vagrant scripts added
- bash-completion for shells add
- cron job scripts to update lookup data added
- logcheck example rules added
- logrotate configuration added


2016/06/18
----------

* improvements in pipeline:
  - PipelineFactory to give possibility to easily add a new broker (Redis, ZMQ, etc..)
  - Splitter feature: if this option is enable, will split the events in source queue to multiple destination queues
* add different messages support:
  - the system is flexible to define a new type of message like 'tweet' without change anything in bot.py, pipeline.py. Just need to add a new class in message.py and harmonization.conf
* add harmonization support
  - in harmonization.conf is possible to define the fields of a specific message in json format.
  - the harmonization.py has data types witch contains sanitize and validation methods that will make sure that the values are correct to be part of an event.
* Error Handling
 - multiple parameters in configuration which gives possibility to define how bot will handle some errors. Example of parameters:
   - `error_procedure` - retry or pass in case of error
   - `error_retry_delay` - time in seconds to retry
   - `error_max_retries` - number of retries
   - `error_log_message` - log or not the message in error log
   - `error_log_exception` - log or not the exception in error log
   - `error_dump_message` - log or not the message in dump log to be fixed and re-insert in pipeline
* Exceptions
  - custom exceptions for IntelMQ
* Defaults configurations
  - new configuration file to specify the default parameters which will be applied to all bots. Bots can overwrite the configurations.
* New bots/feeds


2015/06/03 (aaron)
------------------

  * fixed the license to AGPL in setup.py
  * moved back the documentation from the wiki repo to `docs/`. See #205.
  * added python-zmq as a setup requirement in UserGuide . See #206
