# Geoserver::Publish

![CI](https://github.com/samvera-labs/geoserver-publish/actions/workflows/ruby.yml/badge.svg)
[![Coverage Status](https://img.shields.io/badge/coverage-100%25-brightgreen)](https://github.com/samvera-labs/geoserver-publish/blob/da3eef51705eea8e3c14ff0b88aea70064b979d4/spec/spec_helper.rb#L13)

Simple client for publishing Shapefiles and GeoTIFFs to Geoserver.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'geoserver-publish'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install geoserver-publish

## Usage

#### GeoTIFF

```ruby
Geoserver::Publish.geotiff(workspace_name: "public", file_path: "file:///path/on/geoserver/raster.tif", id: "1234", title: "GeoTiff Title")
```

#### Shapefile

Because of a limitation in GeoServer, the layer id  must be the same name as the shapefile without the extenstion. For example, if the file name is `1234.shp`, the id must be `1234`.

```ruby
Geoserver::Publish.shapefile(workspace_name: "public", file_path: "file:///path/on/geoserver/1234.shp", id: "1234", title: "Shapefile Title")
```

#### Deleting

```Ruby
Geoserver::Publish.delete_geotiff(workspace_name: "public", id: "1234")
Geoserver::Publish.delete_shapefile(workspace_name: "public", id: "1234")
```

#### Styles

For users wanting to publish styles, this can be done using the [GeoServer styles API](https://docs.geoserver.org/latest/en/api/#1.0.0/styles.yaml).

```ruby
Geoserver::Publish::Style.new.create(style_name: "raster_layer", filename: "raster_layer.sld")
sld = File.read("./spec/fixtures/files/payload/raster_layer.sld")
Geoserver::Publish::Style.new.update(style_name: "raster_layer", filename: "raster_layer.sld", payload: sld)
```

#### GeoWebCache

The GeoWebCache REST API can also be used. Note: there is usually a different url that is used.

```ruby
c = Geoserver::Publish::Connection.new({"url" => "http://localhost:8080/geoserver/gwc/rest", "user"=>"admin",
"password"=>"geoserver"})
Geoserver::Publish::Geowebcache.new(c).masstruncate(layer_name: 'nurc:Pk50095')
```

## Configuration

#### Default

To use the default GeoServer connection parameters, no further configuration is needed. These are useful for testing and development against a local GeoServer instance.

 - url: http://localhost:8080/geoserver/rest
 - user: admin
 - password: geoserver


#### Environment Variables

GeoServer Connection parameters can be set using environment variables.

  - GEOSERVER_URL
  - GEOSERVER_USER
  - GEOSERVER_PASSWORD

#### Connection Object

A connection object can be instantiated with a parameter hash and passed into publishing methods.

```ruby
new_conn = Geoserver::Publish::Connection.new({"url"=> "http://mygeoserver.com:8181/geoserver/rest", "user" => "admin_user", "password" => "supersecret"})
Geoserver::Publish.geotiff(connection: new_conn, workspace_name: "public", file_path: "file:///path/on/geoserver/raster.tif", id: "1234", title: "GeoTiff Title")
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/samvera-labs/geoserver-publish.

If you're working on a PR for this project, create a feature branch off of `main`. 

This repository follows the [Samvera Community Code of Conduct](https://samvera.atlassian.net/wiki/spaces/samvera/pages/405212316/Code+of+Conduct) and [language recommendations](https://github.com/samvera/maintenance/blob/main/templates/CONTRIBUTING.md#language).  Please ***do not*** create a branch called `master` for this repository or as part of your pull request; the branch will either need to be removed or renamed before it can be considered for inclusion in the code base and history of this repository.
