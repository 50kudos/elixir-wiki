## Deployment (edeliver + distillery)

#### Initial release
  * [Add edeliver and distillery to mix deps](https://github.com/boldpoker/edeliver#quick-start)
  * `mix do deps.get, compile`
  * `mix release.init`
  * [Create required configuration for edeliver **.deliver/config**](https://github.com/50kudos/elixir-wiki/blob/master/.deliver/config)
    * <details>
      <summary>build_deps.sh</summary>

      ```
      #!/usr/bin/env bash

      [ -f ~/.profile ] && source ~/.profile
      set -e

      echo "Adding Erlang Solutions repo"
      sudo wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
      sudo dpkg -i erlang-solutions_1.0_all.deb

      echo "Updating apt-get"
      sudo apt-get update

      echo "Installing git"
      sudo apt-get install git

      echo "Installing node and npm"
      sudo apt-get -y install nodejs-legacy
      sudo apt-get -y install npm

      echo "Installing the Erlang/OTP platform and all of its applications"
      sudo apt-get -y install esl-erlang

      echo "Installing Elixir"
      sudo apt-get install elixir

      # Database: postgresql
      # Use a SQL service (Google, RDS, etc.)
      ```
    </details>
  * Setup remote database
    * [Enable Cloud SQL API and create a service account](https://cloud.google.com/sql/docs/postgres/connect-admin-proxy)
    * [Add pre boot hook for database connecting **rel/config.exs**](https://github.com/50kudos/elixir-wiki/blob/master/rel/config.exs)
      * <details>
        <summary>sql_cloud_proxy.sh</summary>

        ```
        #!/usr/bin/env bash

        # Database: postgresql
        # Use a SQL service (Google, RDS, etc.)
        #
        # Setup google cloud sql proxy for database remote connecting
        sudo wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64
        mv cloud_sql_proxy.linux.amd64 cloud_sql_proxy
        chmod +x cloud_sql_proxy
        ./cloud_sql_proxy -instances=elixir-wiki-164207:asia-east1:psql-repo=tcp:5432 -credential_file=/root/elixir-wiki-2e73539075e0.json &

        # Export http port for an endpoint
        export PORT=80
        ```
      </details>
  * Commit everything above before building a release.
    * `mix edeliver build release`
    * `mix edeliver deploy release to production`
    * `mix edeliver start production`

#### Regular release flow (No hot code reloading)
  * For non-feature release, run `mix edeliver build release --increment-version=patch`
  * For feature release, run `mix edeliver build release --increment-version=minor`
  * For epic feature (this should be rarely the case), run `mix edeliver build release --increment-version=major`
  * `mix edeliver deploy release to production`
  * `mix edeliver restart production`
