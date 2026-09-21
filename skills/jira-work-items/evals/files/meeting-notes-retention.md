notes from sync w/ legal + platform, tues

- legal: we HAVE to be able to delete docs after retention period, gdpr. period differs per bucket
  (invoices 10y, support attachments 2y, everything else 5y?? legal will confirm by end of month)
- also legal hold: if a doc is under hold it must NOT be deleted even if retention expired
- platform: buckets are on object storage, they can do lifecycle rules natively but only per bucket
  not per doc, so legal hold on single docs wouldn't work with native rules. not sure if we can do tags
- dmitri: maybe simpler to have our own nightly job that scans metadata and deletes. but then audit trail?
  legal wants proof of deletion (who/when/what) kept 7 years
- nobody knows how many docs are past retention today. could be millions. need a number before
  we decide on job vs lifecycle
- admin ui: someone (ADMIN role) needs to set retention per bucket + place/release holds. no design yet
- decision: not doing this for the archive bucket, that one is frozen for the audit
- want first version in Q3, ideally
