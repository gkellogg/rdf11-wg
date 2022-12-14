-# This template is used for generating a rollup EARL report. It expects to be
-# called with a single _tests_ local with the following structure
-#
-#  {
-#    "@context": {...},
-#    "@id": "",
-#    "@type": "earl:Software",
-#    "name": "...",
-#    "bibRef": "[[...]]",
-#    "assertions": ["./rdf.rb-earl.ttl"],
-#    "testSubjects": [
-#      {
-#        "@id": "http://rubygems.org/gems/rdf-turtle",
-#        "@type": "earl:TestSubject",
-#        "name": "RDF::Turtle"
-#      },
-#      ...
-#    ],
-#    "tests": [{
-#      "@id": "http://www.w3.org/2013/TurtleTests/manifest.ttl#turtle-syntax-file-01",
-#      "@type": ["earl:TestCriterion", "earl:TestCase"],
-#      "title": "subm-test-00",
-#      "description": "Blank subject",
-#      "testAction": "http://www.w3.org/2013/TurtleTests/turtle-syntax-file-01.ttl",
-#      "testResult": "http://www.w3.org/2013/TurtleTests/turtle-syntax-file-01.out"
-#      "mode": "earl:automatic",
-#      "assertions": [
-#        {
-#          "@type": "earl:Assertion",
-#          "assertedBy": "http://greggkellogg.net/foaf#me",
-#          "test": "http://svn.apache.org/repos/asf/jena/Experimental/riot-reader/testing/RIOT/Lang/TurtleSubm/manifest.ttl#testeval00",
-#          "subject": "http://rubygems.org/gems/rdf-turtle",
-#          "result": {
-#            "@type": "earl:TestResult",
-#            "outcome": "earl:passed"
-#          }
-#        }
-#      ]
-#    }]
-#  }
- require 'cgi'

!!! 5
%html{:prefix => "earl: http://www.w3.org/ns/earl# doap: http://usefulinc.com/ns/doap# mf: http://www.w3.org/2001/sw/DataAccess/tests/test-manifest#"}
  - subjects = tests['testSubjects']
  %head
    %meta{"http-equiv" => "Content-Type", :content => "text/html;charset=utf-8"}
    %link{:rel => "alternate", :href => "earl.ttl"}
    %link{:rel => "alternate", :href => "earl.jsonld"}
    - tests['assertions'].each do |file|
      %link{:rel => "related", :href => file}
    %title
      = tests['name']
      Implementation Report
    %script.remove{:src => "../../local-biblio.js"}
    %script.remove{:type => "text/javascript", :src => "https://www.w3.org/Tools/respec/respec-w3c-common"}
    :javascript
      var respecConfig = {
          // extend the bibliography entries
          localBiblio: localBibliography,

          // specification status (e.g. WD, LCWD, NOTE, etc.). If in doubt use ED.
          specStatus:           "unofficial",
          copyrightStart:       "2010",
          doRDFa:               "1.1",

          // the specification's short name, as in http://www.w3.org/TR/short-name/
          shortName:            "trig-earl",
          //subtitle:             "TriG Implementation Conformance Report",
          // if you wish the publication date to be other than today, set this
          publishDate:  "#{Time.now.strftime("%Y/%m/%d")}",

          // if there is a previously published draft, uncomment this and set its YYYY-MM-DD date
          // and its maturity status
          //previousPublishDate:  "2011-10-23",
          //previousMaturity:     "ED",
          //previousDiffURI:      "http://json-ld.org/spec/ED/json-ld-syntax/20111023/index.html",
          //diffTool:             "http://www.aptest.com/standards/htmldiff/htmldiff.pl",

          // if there a publicly available Editor's Draft, this is the link
          //edDraftURI:           "",

          // if this is a LCWD, uncomment and set the end of its review period
          // lcEnd: "2009-08-05",

          // editors, add as many as you like
          // only "name" is required
          editors:  [
              { name: "Gregg Kellogg", url: "http://greggkellogg.net/",
                company: "Kellogg Associates" }
          ],

          // authors, add as many as you like.
          // This is optional, uncomment if you have authors as well as editors.
          // only "name" is required. Same format as editors.
          //authors:  [
          //RDF Working Group],

          // name of the WG
          wg:           "RDF Working Group",

          // URI of the public WG page
          wgURI:        "http://www.w3.org/2011/rdf-wg/",

          // name (with the @w3c.org) of the public mailing to which comments are due
          wgPublicList: "public-rdf-comments",

          // URI of the patent status for this WG, for Rec-track documents
          // !!!! IMPORTANT !!!!
          // This is important for Rec-track documents, do not copy a patent URI from a random
          // document unless you know what you're doing. If in doubt ask your friendly neighbourhood
          // Team Contact.
          wgPatentURI:  "http://www.w3.org/2004/01/pp-impl/46168/status",
          alternateFormats: [
            {uri: "earl.ttl", label: "Turtle"},
            {uri: "earl.jsonld", label: "JSON-LD"}
          ],
      };
    :css
      span[property='dc:description'] { display: none; }
      td.PASS { color: green; }
      td.FAIL { color: red; }
      table.report {
        border-width: 1px;
        border-spacing: 2px;
        border-style: outset;
        border-color: gray;
        border-collapse: separate;
        background-color: white;
      }
      table.report th {
        border-width: 1px;
        padding: 1px;
        border-style: inset;
        border-color: gray;
        background-color: white;
        -moz-border-radius: ;
      }
      table.report td {
        border-width: 1px;
        padding: 1px;
        border-style: inset;
        border-color: gray;
        background-color: white;
        -moz-border-radius: ;
      }
      tr.summary {font-weight: bold;}
      td.passed-all {color: green;}
      td.passed-most {color: darkorange;}
      td.passed-some {color: red;}
  %body{:prefix => "earl: http://www.w3.org/ns/earl# doap: http://usefulinc.com/ns/doap# mf: http://www.w3.org/2001/sw/DataAccess/tests/test-manifest#"}
    %section#abstract{:about => tests['@id'], :typeof => [tests['@type']].flatten.join(" ")}
      %p
        This document report test subject conformance for and related specifications for
        %span{:property => "doap:name"}<=tests['name']
        %span{:property => "dc:bibliographicCitation"}<
          = tests['bibRef']
        according to the requirements of the Evaluation and Report Language (EARL) 1.0 Schema [[EARL10-SCHEMA]].
      %p
        This report is also available in alternate formats:
        %a{:rel => "xhv:alternate", :href => "earl.ttl"}
          Turtle
        and
        %a{:rel => "xhv:alternate", :href => "earl.jsonld"}
          JSON-LD
      %p
        See also the
        %a{href: "http://www.w3.org/2013/TrigReports/CR-exit"}
          Implementation report for PR transition
        (a step in the
        %a{href: "http://www.w3.org/2014/Process-20140801/#rec-pr"}<="W3C Process"
        ).
    %section#sodt
    %section
      :markdown
        ## Instructions for submitting implementation reports

          Tests should be run using the test manifests defined in the 
          [Test Manifests](#test-manifests) Section.

          The assumed base URI for the tests is `<http://example/base/>` if needed.

          Reports should be submitted in Turtle format to [public-rdf-comments@w3.org](mailto:public-rdf-comments@w3.org)
          and include an `earl:Assertion`
          for each test, referencing the test resource from the associated manifest
          and the test subject being reported upon. An example test entry is be the following:

              [ a earl:Assertion;
                earl:assertedBy <http://greggkellogg.net/foaf#me>;
                earl:subject <http://rubygems.org/gems/rdf-turtle>;
                earl:test <http://www.w3.org/2013/TurtleTests/manifest.ttl#turtle-syntax-file-01>;
                earl:result [
                  a earl:TestResult;
                  earl:outcome earl:passed;
                  dc:date "2013-02-22T15:12:30-08:00"^^xsd:dateTime];
                earl:mode earl:automatic ] .

          The Test Subject should be defined as a `doap:Project`, including the name,
          homepage and developer(s) of the software (see [[DOAP]]). Optionally, including the
          project description and programming language. An example test subject description is the following:

              <> foaf:primaryTopic <http://rubygems.org/gems/rdf-trig>
                dc:issued "2013-06-18T17:30:22-07:00"^^xsd:dateTime ;
                foaf:maker <http://greggkellogg.net/foaf#me> .

              <http://rubygems.org/gems/rdf-trig> a doap:Project, earl:TestSubject, earl:Software ;
                doap:name          "RDF::TriG" ;
                doap:homepage      <https://ruby-rdf.github.io/rdf-trig> ;
                doap:license       <http://creativecommons.org/licenses/publicdomain/> ;
                doap:description   "RDF::TriG is an TriG reader/writer for the RDF.rb library suite."@en ;
                doap:created       "2011-08-29"^^xsd:date ;
                doap:programming-language "Ruby" ;
                doap:implements    <http://www.w3.org/TR/trig/> ;
                doap:category      <http://dbpedia.org/resource/Resource_Description_Framework>,
                                   <http://dbpedia.org/resource/Ruby_(programming_language)> ;
                doap:developer     <http://greggkellogg.net/foaf#me> ;
                dc:title           "RDF::TriG" ;
                dc:description     "RDF::TriG is an TriG reader/writer for the RDF.rb library suite."@en ;
                dc:date            "2011-08-29"^^xsd:date ;
                .

          The software developer, either an organization or one or more individuals SHOULD be
          referenced from `doap:developer` using [[FOAF]]. For example:

              <http://greggkellogg.net/foaf#me> a foaf:Person, earl:Assertor;
                foaf:name "Gregg Kellogg";
                foaf:title "Implementor";
                foaf:homepage <http://greggkellogg.net/> .

          See [Turtle Test Suite Wiki](http://www.w3.org/2011/rdf-wg/wiki/Turtle_Test_Suite)
          for more information.
    %section
      - test_info = {}
      - test_refs = {}
      - subject_refs = {}
      %h2
        Test Manifests
      - tests['entries'].each do |manifest|
        - test_cases = manifest['entries']
        %section{:typeof => manifest['@type'].join(" "), :resource => manifest['@id']}
          %h2<=manifest['title']
          - [manifest['description']].flatten.compact.each do |desc|
            %p<
              ~ CGI.escapeHTML desc
          %table.report
            - skip_subject = {}
            - passed_tests = []
            %tr
              %th
                Test
              - subjects.each_with_index do |subject, index|
                - subject_refs[subject['@id']] = "subj_#{index}"
                -# If subject is untested for every test in this manifest, skip it
                - skip_subject[subject['@id']] = manifest['entries'].all? {|t| t['assertions'][index]['result']['outcome'] == 'earl:untested'}
                - unless skip_subject[subject['@id']]
                  %th
                    %a{:href => '#' + subject_refs[subject['@id']]}<=subject['name']
            - test_cases.each do |test|
              - tid = 'test_' + (test['@id'][0,2] == '_:' ? test['@id'][2..-1] : test['@id'].split('#').last)
              - (test_info[tid] ||= []) << test
              - test_refs[test['@id']] = tid
              %tr{:rel => "mf:entries", :typeof => test['@type'].join(" "), :resource => test['@id'], :inlist => true}
                %td
                  %a{:href => "##{tid}"}<
                    ~ CGI.escapeHTML test['title']
                - test['assertions'].each_with_index do |assertion, ndx|
                  - next if skip_subject[assertion['subject']]
                  - pass_fail = assertion['result']['outcome'].split(':').last.upcase.sub(/(PASS|FAIL)ED$/, '\1')
                  - passed_tests[ndx] = (passed_tests[ndx] || 0) + (pass_fail == 'PASS' ? 1 : 0)
                  %td{:class => pass_fail, :property => "earl:assertions", :typeof => assertion['@type'], :inlist => true}
                    - if assertion['assertedBy']
                      %link{:property => "earl:assertedBy", :href => assertion['assertedBy']}
                    %link{:property => "earl:test", :href => assertion['test']}
                    %link{:property => "earl:subject", :href => assertion['subject']}
                    - if assertion['mode']
                      %link{:property => 'earl:mode', :href => assertion['mode']}
                    %span{:property => "earl:result", :typeof => assertion['result']['@type']}
                      %span{:property => 'earl:outcome', :resource => assertion['result']['outcome']}
                        = pass_fail
            %tr.summary
              %td
                = "Percentage passed out of #{manifest['entries'].length} Tests"
              - passed_tests.compact.each do |r|
                - pct = (r * 100.0) / manifest['entries'].length
                %td{:class => (pct == 100.0 ? 'passed-all' : (pct >= 95.0 ? 'passed-most' : 'passed-some'))}
                  = "#{'%.1f' % pct}%"
    %section.appendix
      %h2
        Test Subjects
      %p
        This report was tested using the following test subjects:
      %dl
        - subjects.each_with_index do |subject, index|
          %dt{:id => subject_refs[subject['@id']]}
            %a{:href => subject['@id']}
              %span{:about => subject['@id'], :property => "doap:name"}<= subject['name']
          %dd{:property => "earl:testSubjects", :resource => subject['@id'], :typeof => [subject['@type']].flatten.join(" "), :inlist => true}
            %dl
              - if subject['doapDesc']
                %dt= "Description"
                %dd{:property => "doap:description", :lang => 'en'}<
                  ~ CGI.escapeHTML subject['doapDesc']
              - if subject['language']
                %dt= "Programming Language"
                %dd{:property => "doap:programming-language"}<
                  ~ CGI.escapeHTML subject['language']
              - if subject['homepage']
                %dt= "Home Page"
                %dd{:property => "doap:homepage"}
                  %a{:href=> subject['homepage']}
                    ~ CGI.escapeHTML subject['homepage']
              - if subject['developer']
                %dt= "Developer"
                %dd{:rel => "doap:developer"}
                  - subject['developer'].each do |dev|
                    %div{:resource => dev['@id'], :typeof => [dev['@type']].flatten.join(" ")}
                      - if dev.has_key?('@id')
                        %a{:href => dev['@id']}
                          %span{:property => "foaf:name"}<
                            ~ CGI.escapeHTML dev['foaf:name']
                      - else
                        %span{:property => "foaf:name"}<
                          ~ CGI.escapeHTML dev['foaf:name']
                      - if dev['foaf:homepage']
                        %dt
                          Home Page
                        %dd
                          %a{:property => "foaf:homepage", :href=> dev['foaf:homepage']}
                            ~ CGI.escapeHTML dev['foaf:homepage']
              %dt
                Test Suite Compliance
              %dd
                %table.report
                  %tbody
                    - tests['entries'].each do |manifest|
                      - passed = manifest['entries'].select {|t| t['assertions'][index]['result']['outcome'] == 'earl:passed' }.length
                      - next if passed == 0
                      - total = manifest['entries'].length
                      - pct = (passed * 100.0) / total
                      %tr
                        %td{:class => (pct == 100.0 ? 'passed-all' : (pct >= 85.0 ? 'passed-most' : 'passed-some'))}
                          = "#{passed}/#{total} (#{'%.1f' % pct}%)"
    - unless tests['assertions'].empty?
      %section.appendix{:rel => "xhv:related earl:assertions"}
        %h2
          Individual Test Results
        %p
          Individual test results used to construct this report are available here:
        %ul
          - tests['assertions'].each do |file|
            %li
              %a.source{:href => file}<= file
    %section.appendix
      %h2
        Test Definitions
      %dl
        - tests['entries'].each do |manifest|
          %div{:property => "mf:entries", :inlist => true, :resource => manifest['@id']}
            - manifest['entries'].each do |test|
              %dt{:id => test_refs[test['@id']], :resource => test['@id']}
                Test
                %span{:property => "dc:title mf:name"}<
                  ~ CGI.escapeHTML test['title']
              %dd{:resource => test['@id']}
                %p{:property => "dc:description", :lang => 'en'}<
                  ~ CGI.escapeHTML test['description']
                %pre{:class => "example actionDoc", :property => "mf:action", :resource => test['testAction'], :title => "#{test['title']} Input"}<
                  ~ Kernel.open(test['testAction']) {|f| f.set_encoding(Encoding::UTF_8); CGI.escapeHTML(f.read).gsub(/\n/, '<br/>')} rescue "#{test['testAction']} not loaded"
                - if test['testResult']
                  %pre{:class => "example resultDoc", :property => "mf:result", :resource => test['testResult'], :title => "#{test['title']} Result"}<
                    ~ Kernel.open(test['testResult']) {|f| f.set_encoding(Encoding::UTF_8); CGI.escapeHTML(f.read).gsub(/\n/, '<br/>')} rescue "#{test['testResult']} not loaded"
    %section#appendix{:property => "earl:generatedBy", :resource => tests['generatedBy']['@id'], :typeof => tests['generatedBy']['@type']}
      %h2
        Report Generation Software
      - doap = tests['generatedBy']
      - rel = doap['release']
      %p
        This report generated by
        %span{:property => "doap:name"}<
          %a{:href => tests['generatedBy']['@id']}<
            = doap['name']
        %meta{:property => "doap:shortdesc", :content => doap['shortdesc'], :lang => 'en'}
        %meta{:property => "doap:description", :content => doap['doapDesc'], :lang => 'en'}
        version
        %span{:property => "doap:release", :resource => rel['@id'], :typeof => 'doap:Version'}
          %span{:property => "doap:revision"}<=rel['revision']
          %meta{:property => "doap:name", :content => rel['name']}
          %meta{:property => "doap:created", :content => rel['created'], :datatype => "xsd:date"}
        an
        %a{:property => "doap:license", :href => doap['license']}<="Unlicensed"
        %span{:property => "doap:programming-language"}<="Ruby"
        application. More information is available at
        %a{:property => "doap:homepage", :href => doap['homepage']}<=doap['homepage']
        = "."
      %p{:property => "doap:developer", :resource => "http://greggkellogg.net/foaf#me", :typeof => "foaf:Person"}
        This software is provided by
        %a{:property => "foaf:homepage", :href => "http://greggkellogg.net/"}<
          %span{:aboue => "http://greggkellogg.net/foaf#me", :property => "foaf:name"}<
            Gregg Kellogg
        in hopes that it might make the lives of conformance testers easier.
