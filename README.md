# 剧本杀搜线索程序-Python
# 剧本杀《暗影计划》六人公开本，普通线索1*6，深入调查线索1*6，密令6

from textwrap import dedent
from sys import exit
 
# 人物姓名、剩余AP数、剩余线索数对应字典
passwords = {'2':'陈年', '3':'夜酒', '5':'江岚', '7':'南枫', '11':'瞿春烟', '13':'连君'}
AP_number = {'陈年':5, '夜酒':5, '江岚':5, '南枫':5, '瞿春烟':5, '连君':5}
common_clue_left = {'陈年':3, '夜酒':3, '江岚':3, '南枫':3, '瞿春烟':3, '连君':3}
special_clue_left = {'陈年':3, '夜酒':3, '江岚':3, '南枫':3, '瞿春烟':3, '连君':3}

common_clues = {'陈年1':'内容', '夜酒1':'内容', '江岚1':'内容', '南枫1':'内容', '瞿春烟1':'内容', '连君1':'内容',
                '陈年2':'内容', '夜酒2':'内容', '江岚2':'内容', '南枫2':'内容', '瞿春烟2':'内容', '连君2':'内容',
                '陈年3':'内容', '夜酒3':'内容', '江岚3':'内容', '南枫3':'内容', '瞿春烟3':'内容', '连君3':'内容'}

special_clues = {'陈年1':'内容', '夜酒1':'内容', '江岚1':'内容', '南枫1':'内容', '瞿春烟1':'内容', '连君1':'内容',
                 '陈年2':'内容', '夜酒2':'内容', '江岚2':'内容', '南枫2':'内容', '瞿春烟2':'内容', '连君2':'内容',
                 '陈年3':'内容', '夜酒3':'内容', '江岚3':'内容', '南枫3':'内容', '瞿春烟3':'内容', '连君3':'内容'}

key_word_code = {'未亡人':6 ,'离心仆': 15,'恨生时': 77, '牵机变': 14, '善恶果': 143, '飘摇雪':65}
key_words = {6:'aa', 15:'bb', 77:'cc', 14:'dd', 143:'ee', 65:'ff'}
key_words_copy = {6:'aa', 15:'bb', 77:'cc', 14:'dd', 143:'ee', 65:'ff'}
key_word_chance = {'陈年':3, '夜酒':3, '江岚':3, '南枫':3, '瞿春烟':3, '连君':3}


def check_one(name):
    AP_number[name]
    print(f"{name}你好, 你还有{AP_number[name]}个AP点。")
    if AP_number[name] >1:
        print("每条普通线索消耗1个AP。每条深入调查线索消耗2个AP。你不可以搜自己的线索。")
        return
    elif AP_number[name] > 0:
       print("每条普通线索消耗1个AP。你不可以搜自己的线索。")
       return 
    else:
        print("你已经不可以进行单人搜索了。")
        return

def check_two(name1, name2):
    key_left1 = key_word_chance[name1]
    key_left2 = key_word_chance[name2]
    print(dedent(f"""
        {name1}你好, 你还有{key_left1}个密令机会。
        {name2}你好, 你还有{key_left2}个密令机会。
        """))
    if key_left1 <1 :
        print(f"{name1}, 你没有密令机会了。")
        start()
    elif key_left2 <1 :
        print(f"{name2}, 你没有密令机会了。")
        start()
    else:
        return
    
def common_search():
    password = input("请说出你的个人密码：")
    if password not in passwords.keys():
        common_search()
    else:
        name = passwords[password]
        search(name)
        
def search(name) :
    check_one(name)
    if AP_number[name] > 0:
        _name = input ("请输入要搜索的中文姓名：")
        if _name not in common_clue_left.keys():
            search(name)
        elif _name == name:
            print("你不可以搜索自己的线索。")
            search(name)
        else:
            print(dedent(f"""关于{_name}有{common_clue_left[_name]}条普通线索，请问你想搜第几条："""))
            input_num = input ()
            if input_num not in ['1','2','3']:
                search(name)
            else:
                common_clue_num = _name + input_num
                print(common_clue_num, ':', common_clues[common_clue_num])
                AP_number[name]-=1
                check_one(name)
                if AP_number[name] >1:
                    special = input("是否就此条线索进行深入调查，请回答'是' 或 '否':")
                    if special == '是':
                        print(common_clue_num, '深入调查:', special_clues[common_clue_num])
                        AP_number[name]-=2
                        check_one(name)
                        search(name)
                    else:
                        search(name)
                elif AP_number[name]>0:
                    search(name)
                else:
                    start()
    else:
        start()    

def code_search():
    code1 = input("请输入第一个人的密码：")
    code2 = input ("请输入第二个人的密码：")
    if code1 not in passwords.keys() or code2 not in passwords.keys():
        code_search()
    else:
        name1 = passwords[code1]
        name2 = passwords[code2]
        check_two(name1,name2)
        code = int(code1) * int(code2)
        key_word = input("请说出密令：")
        key_word_chance[name1]-=1
        key_word_chance[name2]-=1
        if key_word not in key_word_code.keys():
            print("没有这个密令，你们各失去一次密令机会。")
        elif key_word_code[key_word] == code:
            if key_words_copy[code]=='已经用过':
                print("这条密令你们俩已经用过了。")
            else:
                print("\n密令内容是：",key_words_copy[code])
                AP_number[name1]+=2
                AP_number[name2]+=2
                key_words_copy[code]='已经用过'
        else:
            print("这不是你们俩的密令，无法打开。")
        print(dedent(f"""
            {name1}, 你还有{AP_number[name1]}个AP点,{key_word_chance[name1]}个密令机会。
            {name2}, 你还有{AP_number[name2]}个AP点,{key_word_chance[name1]}个密令机会。
            """))
    start() 


def start():
    choice = input("请问你想搜查单人线索‘1’，还是双人线索‘2’：")
    if choice != '1' and choice !='2':
        start()
    if choice == '1':
        common_search()
    if choice == '2':
        code_search()

start()
