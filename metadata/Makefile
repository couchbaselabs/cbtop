# Relationship between files...
#   stats.org
#     ==> [automated cleansing (make stats.org.gen)] ==>
#   stats.org.gen
#     ==> [manual annotation (cp stats.org.gen stats.src && emacs stats.src)] ==>
#   stats.src
#     ==> [json conversion (stats-conv stats.src)] ==>
#   stats.json
#
all: stats.org.gen stats.json ns_server.json

clean:
	rm -f stats.org stats.org.gen stats.json ns_server.json

stats.org:
	wget https://raw.github.com/couchbase/ep-engine/master/docs/stats.org

stats.org.gen: stats.org
	cat stats.org | \
      grep -E "(^[|*])|(engine:)" | grep -v "\-\-\-\-\-\-\-" | \
	  grep -v "Stats Definitions" | grep -v "Getting Started" | grep -v "Description" \
    > stats.org.gen

stats.json: stats.src
	./stats-conv stats.src > stats.json

diff:
	grep -v "^  " stats.src | diff stats.org.gen -

test-stats.json.sample: stats.json.sample
	cat stats.json.sample | python -m json.tool

test-stats.json: stats.json
	cat stats.json | python -m json.tool

ns_server.json:
	./visit-conv > ns_server.json
