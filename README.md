# Migration manual proto type hint to mypy-protobuf

### Step 1. Install mypy-protobuf
```
pip install mypy-protobuf==3.2.0
```

### Step 2. Add to protgen 
Add ``--mypy_out="${PROJECT_DIR}"/genproto`` to proto.sh file

```
python -m grpc_tools.protoc -I "${PROJECT_DIR}" -I "${PWD}" --mypy_out="${PROJECT_DIR}"/genproto --python_out="${PROJECT_DIR}"/genproto broker/*.proto
python -m grpc_tools.protoc -I "${PROJECT_DIR}" -I "${PWD}" --mypy_out="${PROJECT_DIR}"/genproto --python_out="${PROJECT_DIR}"/genproto broker/purchase_order/*.proto
....
```

### Step 3. Remove custom typehints
if you have custom type hint like this
```py
class CompanyData(typing.NamedTuple):
    pass

async def company_create_or_update(data):
    company_pb: CompanyData = pb.Company().FromString(data)
    company = await models.Company.raw_objects().get(models.Company.id == company_pb.id)
```

change to 

```py
async def company_create_or_update(data):
    company_pb = pb.Company().FromString(data)
    company = await models.Company.raw_objects().get(models.Company.id == company_pb.id)
```
