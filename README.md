
# flask-neomodel

An attempt to produce a flask plugin for [`neomodel`](http://neomodel.readthedocs.io/en/latest/) class mapping of GraphDB objects.

---

Basic setup:

```bash
# install the library
pip3 install git+https://github.com/pdonorio/flask-neomodel.git@master

# launch a graphdb with docker
docker run --rm \
    -p 7474:7474 -p 7687:7687 -e NEO4J_AUTH=neo4j/test \
    neo4j:3.3.5

```

Then you can test this snippet:
```python
from flask import Flask
from flask_neomodel import NeoModel

app = Flask('neomodel APIs')
neomodel = NeoModel(app)


class Test(neomodel.neo.StructuredNode):
    name = neomodel.neo.StringProperty(unique_index=True)


@app.route('/')
def test_neomodel():
    app.logger.debug("graph db object: %s" % neomodel.graph_db)
    test = Test(name="just a test").save()  # Create a new node
    app.logger.info("created test object: %s" % test)
    return 'Hello World!'


if __name__ == "__main__":
    app.run(host='0.0.0.0', debug=True)
```
