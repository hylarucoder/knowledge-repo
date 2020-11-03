# SQLAlchemy Core

## CheatSheet

```python
# users table
ins = users.insert()
# 'INSERT INTO users (id, name, fullname) VALUES (:id, :name, :fullname)'

ins = users.insert().values(name='jack', fullname='Jack Jones')
# 'INSERT INTO users (name, fullname) VALUES (:name, :fullname)'

ins.compile().params
# {'fullname': 'Jack Jones', 'name': 'jack'}

conn = engine.connect()
# <sqlalchemy.engine.base.Connection object at 0x...>

ins.bind = engine

result = conn.execute(ins)
result.inserted_primary_key

conn.execute(addresses.insert(), [
   {'user_id': 1, 'email_address' : 'jack@yahoo.com'},
   {'user_id': 1, 'email_address' : 'jack@msn.com'},
   {'user_id': 2, 'email_address' : 'www@www.org'},
   {'user_id': 2, 'email_address' : 'wendy@aol.com'},
])
```

### Select

```python
from sqlalchemy.sql import select

# select * from users
s = select([users])

result = conn.execute(s)
row = result.fetchone()
print("name:", row['name'], "; fullname:", row['fullname'])
print("name:", row[1], "; fullname:", row[2])
for row in conn.execute(s):
    print("name:", row[users.c.name], "; fullname:", row[users.c.fullname])

# select name, fullname from users
s = select([users.c.name, users.c.fullname])
# select from where
s = select([users, addresses]).where(users.c.id == addresses.c.user_id)
# NOTE: <sqlalchemy.sql.elements.BinaryExpression object at 0x...>
users.c.id == addresses.c.user_id
# NOTE
str(users.c.id == addresses.c.user_id)
# 'users.id = addresses.user_id'

from sqlalchemy.sql import and_, or_, not_
print(and_(
        users.c.name.like('j%'),
        users.c.id == addresses.c.user_id,
        or_(
             addresses.c.email_address == 'wendy@aol.com',
             addresses.c.email_address == 'jack@yahoo.com'
        ),
        not_(users.c.id > 5)
      )
 )

# users.name LIKE :name_1 AND users.id = addresses.user_id AND
# (addresses.email_address = :email_address_1
#    OR addresses.email_address = :email_address_2)
# AND users.id <= :id_1

print(users.c.name.like('j%') & (users.c.id == addresses.c.user_id) &
    (
      (addresses.c.email_address == 'wendy@aol.com') | \
      (addresses.c.email_address == 'jack@yahoo.com')
    ) \
    & ~(users.c.id>5)
)
# users.name LIKE :name_1 AND users.id = addresses.user_id AND
# (addresses.email_address = :email_address_1
#     OR addresses.email_address = :email_address_2)
# AND users.id <= :id_1

s = select([(users.c.fullname +
              ", " + addresses.c.email_address).
               label('title')]).\
       where(
          and_(
              users.c.id == addresses.c.user_id,
              users.c.name.between('m', 'z'),
              or_(
                 addresses.c.email_address.like('%@aol.com'),
                 addresses.c.email_address.like('%@msn.com')
              )
          )
       )
conn.execute(s).fetchall()

# SELECT users.fullname || ? || addresses.email_address AS title
# FROM users, addresses
# WHERE users.id = addresses.user_id AND users.name BETWEEN ? AND ? AND
# (addresses.email_address LIKE ? OR addresses.email_address LIKE ?)
# (', ', 'm', 'z', '%@aol.com', '%@msn.com')

s = select([(users.c.fullname +
              ", " + addresses.c.email_address).
               label('title')]).\
       where(users.c.id == addresses.c.user_id).\
       where(users.c.name.between('m', 'z')).\
       where(
              or_(
                 addresses.c.email_address.like('%@aol.com'),
                 addresses.c.email_address.like('%@msn.com')
              )
       )
conn.execute(s).fetchall()

# SELECT users.fullname || ? || addresses.email_address AS title
# FROM users, addresses
# WHERE users.id = addresses.user_id AND users.name BETWEEN ? AND ? AND
# (addresses.email_address LIKE ? OR addresses.email_address LIKE ?)
# (', ', 'm', 'z', '%@aol.com', '%@msn.com')

s = select([
       text("users.fullname || ', ' || addresses.email_address AS title")
    ]).\
        where(
            and_(
                text("users.id = addresses.user_id"),
                text("users.name BETWEEN 'm' AND 'z'"),
                text(
                    "(addresses.email_address LIKE :x "
                    "OR addresses.email_address LIKE :y)")
            )
        ).select_from(text('users, addresses'))
conn.execute(s, x='%@aol.com', y='%@msn.com').fetchall()

# SELECT users.fullname || ', ' || addresses.email_address AS title
# FROM users, addresses
# WHERE users.id = addresses.user_id AND users.name BETWEEN 'm' AND 'z'
# AND (addresses.email_address LIKE ? OR addresses.email_address LIKE ?)
# ('%@aol.com', '%@msn.com')
```


