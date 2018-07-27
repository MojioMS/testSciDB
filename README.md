# testSciDB

# 0) Prerequirements: install scidb or run the rvernica image, move the testfile.out into the scidb directory:

docker run -p 8080:8080 --name scidb rvernica/scidb:16.9-pkg

docker cp testfile.out scidb:/opt/scidb/16.9/DB-scidb/0/0/testfile.out




# 1) Create 3-dim array from file, save the outcome of a between operation as csv file

http://localhost:8080/new_session

sessionId should be: jy9757acx1eve7nmoigsz8fxqqqvjj5i, if not: adjust further &id= params

http://localhost:8080/execute_query?query=create array base <r:uint8 null, g:uint8 null, b:uint8, a:uint8> [t=0:*:0:1; x=0:2999:0:5000; y=0:2999:0:5000]&id=jy9757acx1eve7nmoigsz8fxqqqvjj5i

http://localhost:8080/execute_query?query=load(base,'testfile.out', -2, '(uint8 null, uint8 null, uint8 null, uint8 null)')&id=jy9757acx1eve7nmoigsz8fxqqqvjj5i

http://localhost:8080/execute_query?query=save(between(base, 0, 50, 50, 0, 400, 150),'base.csv', -2, 'csv')&id=jy9757acx1eve7nmoigsz8fxqqqvjj5i

docker cp scidb:/opt/scidb/16.9/DB-scidb/0/0/base.csv base.csv


# 2) do the same as in 1) but use different chunk size when creating the 3-dim array

http://localhost:8080/new_session

sessionId should be: iu531gvyx99bwdekbutaj889ee4dx9wf, if not: adjust further &id= params

http://localhost:8080/execute_query?query=create array base2 <r:uint8 null, g:uint8 null, b:uint8, a:uint8> [t=0:*:0:1; x=0:2999:0:400; y=0:2999:0:400]&id=iu531gvyx99bwdekbutaj889ee4dx9wf

http://localhost:8080/execute_query?query=load(base2,'testfile.out', -2, '(uint8 null, uint8 null, uint8 null, uint8 null)')&id=iu531gvyx99bwdekbutaj889ee4dx9wf

http://localhost:8080/execute_query?query=save(between(base2, 0, 50, 50, 0, 400, 150),'base2.csv', -2, 'csv')&id=iu531gvyx99bwdekbutaj889ee4dx9wf

docker cp scidb:/opt/scidb/16.9/DB-scidb/0/0/base2.csv base2.csv



# Expected Result: 
base.csv and base2.csv have identical entries.
# Actual Result: 
Lots of divergent cells
