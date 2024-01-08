---
layout: post
title: SQLAlchemy - relationship
category: Backend
tag: sqlalchemy
---

1. 1:N

```python

class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    user_name = Column(String, nullable=False)
    created_at = Column(DateTime, default=func.now())
    accessed_at = Column(TIMESTAMP, default=func.now(), onupdate=func.current_timestamp())
    description = Column(String, nullable=True)

class Project(Base):
    __tablename__ = 'project'
    id = Column(Integer, primary_key=True, autoincrement=True)
    project_name = Column(String, nullable=False)
    created_at = Column(DateTime, default=func.now())
    accessed_at = Column(TIMESTAMP, default=func.now(), onupdate=func.current_timestamp())
    description = Column(String, nullable=True)

    user_name = Column(String, ForeignKey("user.user_name", ondelete="CASCADE"))
    # user = relationship("User", backref=sqlalchemy.orm.backref("project", passive_deletes=True)) ## not working
    user = relationship("User", backref=sqlalchemy.orm.backref("project", cascade="all,delete"))
```

> `user = relationship("User", backref=sqlalchemy.orm.backref("project", passive_deletes=True))`는 부모와 자식에서 한 곳에서만 삭제가 동작함

> `ForeignKey()`에서 참조하는 부모의 변수는 `primary_key`로 등록되어 있는 변수를 사용해야 한다. 그렇지만 않으면, 복잡한

`backref`와 `back_populates`는 동일한 기능으로 부모와 자식의 관계성을 정의한다. 하지만, `delete`를 하는 과정에서 부모와 자식간의 관게에서 모두 적용되도록 하기 위해서는 `backref`를 추천한다.

    * `backref`와 `back_populates`의 차이는 단순히 `backref`는 부모와 자식간의 관계를 정의하는 데에 있어서 부모나 자식 중 한 곳에서만 정의하면 되는 것이고, `back_populates`는 부모와 자식 모두 정의해 주어야 한다는 것 뿐이다.

    * `delete`를 하는 기능에서 위와 같이 `backref`를 사용한다면, `sqlalchemy.orm.backref("project", cascade="all,delete")`를 통해서 `CACADE` 기능을 활용해야 하고, `ForeignKey`에서도 명시해주어야 동작한다.

    * 자식 class에서 A = `relationship("B", backref=sqlalchemy.orm.backref("C", cascade="all,delete"))`를 선언할 경우:
        * A: 자식 class에서 부모 class를 참조할 수 있는 변수명
        * B: 부모 class명
        * C: 부모 class에서 자식 class를 참조할 수 있는 변수명

### references

1. https://minorman.tistory.com/204
