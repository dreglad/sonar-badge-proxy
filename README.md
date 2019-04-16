Sonar Badge Proxy
=================

The release of _SonarQube_ 7.1 included an [API for _Project Badges_][API] for public repositories.
[Allow usage of project badges on private projects][MMF-1178] is not yet specified or possible.

The _Sonar Badge Proxy_ enables the use of _Project Badges_ with private projects.
It provides a _reverse proxy_ to authenticate the call to the _SonarQube_ instance.


Usage
-----

URL to access a specific BADGE for a PROJECT:

    sbp.localhost/$BADGE/$PROJECT

URL to access a BADGE for specific BRANCH of a PROJECT:

    sbp.localhost/$BADGE/$PROJECT?branch=$BRANCH


### Metric mapping

The BADGE path segment does not always match the metric name used with the [API].

    status          → alert_status
    bugs            → bugs
    codesmells      → code_smells
    coverage        → coverage
    duplications    → duplicated_lines_density
    lines           → ncloc
    maintainability → sqale_rating
    reliability     → reliability_rating
    security        → security_rating
    techdept        → sqale_index
    vulnerabilities → vulnerabilities


### Environment variables

#### PORT
The port the reverse proxy server starts on

#### AUTHORIZATION
The value passed as Authorization header

#### METRIC
A comma separated list of metrics to expose as BADGE

#### REMOTE
The host of the _SonarQube_ installation

#### SECRET
A secret to create a project access token with


### Project Access Token

Access to the badges provided by _Sonar Badge Proxy_ can be restricted.

     assert token == md5("$PROJECT:$SECRET")

The `token` should be provided as a query parameter.


Example
-------

Assume a _SonarQube_ _project_ on `sonar.localhost`.
To access badges for the _bugs_ and _lines_ metrics for master publicly,
start the proxy as follows:

    #!/usr/bin/env bash
    export PORT=4000
    export METRIC=bugs,lines
    export REMOTE=sonar.localhost
    export SECRET=abc123
    ./sonar-badge-proxy

The badges can be accessed through an URL like:

    localhost:4000/coverage/project?branch=master&token=7d9ccf5d9de733c1f7aded0048739e89


License
-------
    
    Copyright (C) 2019  tynn

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU Affero General Public License as
    published by the Free Software Foundation, either version 3 of the
    License, or (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU Affero General Public License for more details.

    You should have received a copy of the GNU Affero General Public License
    along with this program.  If not, see <https://www.gnu.org/licenses/>.


[API]: https://next.sonarqube.com/sonarqube/web_api/api/project_badges/measure
[MMF-1178]: https://jira.sonarsource.com/browse/MMF-1178
