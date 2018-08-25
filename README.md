# ibuilder
from flask import Flask,request,render_template
import pymysql
import traceback
app = Flask(__name__)


@app.route('/')
def hello_world():
    return render_template('login.html')
@app.route('/regist')
def regist():
    return render_template('regist.html')
@app.route('/registuser')
def getRigstRequest():
    db=pymysql.co2nnect("localhost","root","123456","TESTDB")
    cursor=db.cursor()
    sql="INSERT INTO user(user,password)VALUES ("+request.args.get('user')+","+request.args.get('password')+")"
    try:
        cursor.execute(sql)
        db.commit()
        return render_template('login.html')
    except:
        traceback.print_exc()
        db.rollback()
        return '注册失败'
    db.close()
@app.route('/login')
def getLoginRequest():
    db=pymysql.connect("localhoot","root","123456","TESTDB")
    cursor=db.cursor()
    sql="select*from user where user="+request.args.get('user')+"and password="+request.args.get('password')+""
    try:
        cursor.execute(sql)
        results=cursor.fetchall()
        print(len(results))
        if len(results)==1:
            return '登录成功'
        else:
            return '用户名或密码不正确'
        db.commit()
    except:
        traceback.print_exc()
        db.rollback
if __name__=='__main__':
    app.run()
