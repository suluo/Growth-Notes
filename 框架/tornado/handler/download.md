```py
from pymongo import MongoClient
from openpyxl import Workbook
import sys
import xlwt
import StringIO


class DemoHandler(tornado.web.RequestHandler):
    def __init__(self, application, request, *kwargs):
        super(DemoHandler, self).__init__(application, request)
        self.logger = self.application.logger
        self.client = MongoClient(host="127.0.0.1", port=27017).db

    @tornado.web.asynchronous
    @tornado.gen.coroutine
    def get(self, ):
        mo_query = {}
        try:
            table = self.client.collection_names()[0]
            result_num = self.client[table].find(mo_query).count()
            self.logger.info("")
        except Exception, e:
            self.logger.error("DB fail")
        else:
            head = []
            items = self.client[table].find(mo_query, {"_id": 0})
            self.set_header('Content-Type', 'application/x-xls')
            self.set_header('Content-Disposition', 'attachment; filename='+"file.xlsx")
            # sio = yield self.generate_excel(head, items)
            sio = yield self.generate_xlsx(head, items)
            # sio = self.generate_xlsx(head, items)
            # print sys.__getframe().f_lineno, "finish generate *********************"
            self.write(sio.getvalue())
        finally:
            self.finish()

    @tornado.gen.coroutine
    def generate_excel(self, head, items):
        style_bold = xlwt.easyxf('font:bold 1')
        workbook = xlwt.Workbook(encoding='utf-8')
        workbook.country_code = 86
        for key, value in items.items():
            worksheet = workbook.add_sheet(key, cell_overwrite_ok=False)  # True 则可以覆盖
            worksheet.fit_width_to_pages = True
            for col in range(0, 4):
                worksheet.write(0, col, head[col], style_bold)
            # 自定义函数
            # WriteSheetRow(worksheet, head, 0, True)
            # for row in range(1, len(items) + 1):
            #     ValueList = [value].extend(items[row-1])
            #     WriteSheetRow(worksheet, ValueList, row, False)
            for row in range(1, len(value)+1):
                worksheet.write(row, 0, key, style_bold)
                for col in range(1, 4):
                    worksheet.write(row, col, value[row-1][col-1], style_bold)
        sio = StringIO.StringIO()
        workbook.save(sio)
        raise gen.Return(sio)

    @tornado.gen.coroutine
    def generate_xlsx(self, head, items):
        workbook = Workbook()
        workbook.country_code = 86
        i = 0

        for key, valuelist in items.items():
            sheet = workbook.create_sheet(key, i)
            sheet.append(head)
            for row in range(1, len(valuelist)+1):
                item_row = [key] + valuelist[row-1]
                try:
                    sheet.append(item_row)
                except Exception, e:
                    self.logger.error("%s-%s" % (json.dumps(item_row, encoding='gbk'), e), exc_info=True)
                i += 1
        sio = StringIO.StringIO()
        workbook.save(sio)
        raise gen.Return(sio)
        # return sio
```



