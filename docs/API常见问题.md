<font size="6">**东方财富证券极速交易系统交易API常见问题整理**</font>

<table><tbody>
    <tr><font size="5">API应用常见问题</font></tr>
    <tr><td bgcolor="Gainsboro">Q1. 接口是否为异步的？</td></tr>
    <tr><td>A1. API中登录Login()接口为同步的，返回后，可以视为已经登录成功。其余所有接口均为异步的。</td></tr>
    <tr><td bgcolor="Gainsboro">Q2. 当调用接口失败时，如何知道失败原因？</td></tr>
    <tr><td>A2. 可以调用api的接口GetApiLastError()来获取失败原因。</td></tr>
    <tr><td bgcolor="Gainsboro">Q3. CreateTraderApi()中的client_id可以相同么？</td></tr>
    <tr><td>A3. 可以。对于同一account，相同的client_id只能保持一个session连接，后面的登录在前一个session存续期间，无法连接。</td></tr>
    <tr><td bgcolor="Gainsboro">Q4. CreateTraderApi()中的save_file_path必须输入么？</td></tr>
    <tr><td>A4. 必须输入，而且必须是一个有可写权限的实际存在的路径。</td></tr>
    <tr><td bgcolor="Gainsboro">Q5. 盘中订单手续费去哪里查询</td></tr>
    <tr><td>A5. 盘中是预扣手续费。资金查询，返回的预扣资金，包括购买股票未成交时预扣的交易资金+预扣手续费。实盘环境下以盘后清算为准。</td></tr>
    <tr><td bgcolor="Gainsboro">Q6. 同一个账户account可以多个客户端同时登陆么？</td></tr>
    <tr><td>A6. 对于同一账户account的连接，由api的client_id来进行区分。同一个account，可以同时由不同的client_id来进行登陆连接，但是对于同一对account和client_id，一次只能有一个连接，client_id建议选择1~100以内，超过100的，属于API预使用区间，client_id的总使用个数建议不超过5个。</td></tr>
    <tr><td bgcolor="Gainsboro">Q7. 同一个api支持多个账号account登录么？</td></tr>
    <tr><td>A7. 支持。用户在登录后会得到一个session_id，用户需要记录下account对应的session_id，所有的接口函数都要用到session_id。</td></tr>
    <tr><td bgcolor="Gainsboro">Q8. 连接成功后的session会失效么？</td></tr>
    <tr><td>A8. 在同一个交易日内，如果没有出现断线，api也没有销毁，此次的session不会失效。当然，如果用户在登录后，因为阻塞或者其他什么原因导致心跳包无法按时发送到服务器，那么当其他用户用相同的account和client_id再次登录时，前一个session可视为失效。</td></tr>
    <tr><td bgcolor="Gainsboro">Q9. 在响应函数中为何不很快返回有可能导致断线？</td></tr>
    <tr><td>A9. 在服务器向客户端发送的数据量很大的情况下，当用户在响应函数中处理过慢，会导致数据接收缓冲区被填满，服务器无法向客户端发送数据，此时会触发断线。所以在实际使用中，请尽快从响应函数中返回。</td></tr>
    <tr><td bgcolor="Gainsboro">Q10. api会自动重连么？</td></tr>
    <tr><td>A10. api不会自动帮用户重连。用户可以在收到断线通知OnDisconnect()后选择不销毁api，不登出，继续登录login()，此时交易服务器会在用户重新登录后，从断点消息处续传。注意：行情服务器在断线后不会重新推送行情，除非用户重新订阅。</td></tr>
    <tr><td bgcolor="Gainsboro">Q11. 同一个进程中支持几个api？</td></tr>
    <tr><td>A11. 同一个进程中只允许存在一个api。当用户多账户登录时，请确保程序中只创建了一次api，即CreateTraderApi一次。</td></tr>
    <tr><td bgcolor="Gainsboro">Q12. 平台系统支持过夜么？</td></tr>
    <tr><td>A12. 不支持过夜，只支持当日交易时间段。</td></tr>
    <tr><td bgcolor="Gainsboro">Q13. login时选择Restart和Quick公共流订阅方式有什么区别？</td></tr>
    <tr><td>A13. 公共流订阅方式必须在login之前设定，在login之后生效。Restart方式会将今日所有的公共流消息都重新发送一遍。Quick方式登录后，客户端只会收到登录后的一系列公有流消息。</td></tr>
    <tr><td bgcolor="Gainsboro">Q14. 订单响应在哪几种状态下会推送？</td></tr>
    <tr><td>A14. 对于一个订单而言，OnOrderEvent()中只会推送未成交、全成、部撤、全撤、废单5种状态，部成状态不会推送，可以由用户通过订单相关的成交回报OnTradeEvent()来确认订单的部成状态。</td></tr>
    <tr><td bgcolor="Gainsboro">Q15. 调用查询接口，消息返回时，数据是按批过来的么？</td></tr>
    <tr><td>A15. 所有查询数据都是按个推送的，每次推送一个，即当查询结果有N个数据时，会调用N次接口，当最后一个数据推送时，会设置参数is_last为true。</td></tr>
    <tr><td bgcolor="Gainsboro">Q16. 为什么我没有做过撤单操作，可是最后订单被撤了？</td></tr>
    <tr><td>A16. 当发生这种情况时，先确认没有其他人使用与你同样的account登录并操作。API平台支持同一account以不同的client_id登录，如果多人同时登录，可能会互相撤单。</td></tr>
    <tr><td bgcolor="Gainsboro">Q17. 调用InsertOrder()后，返回的order_emt_id是唯一的么？</td></tr>
    <tr><td>A17. 在同一交易日内，保证唯一。所有数据均当日有效，不保证不同交易日唯一。</td></tr>
    <tr><td bgcolor="Gainsboro">Q18. 成交回报有唯一标识么？</td></tr>
    <tr><td>A18. report_index+market字段可以组成唯一标识表示成交回报。</td></tr>
    <tr><td bgcolor="Gainsboro">Q19. 如何检查自成交？</td></tr>
    <tr><td>A19. 对于上交所，exec_id可以唯一标识一笔成交。当发现2笔成交回报拥有相同的exec_id，则可以认为此笔交易自成交了。对于深交所，exec_id是唯一的，暂时无此判断机制。</td></tr>
    <tr><td bgcolor="Gainsboro">Q20. 在断线后，login()之前，调用登出logout()和不调用有何区别？</td></tr>
    <tr><td>A20. 如果不登出就login()，公共流订阅方式不会起作用。用户只会收到断线后的所有消息。如果先logout()再login()，那么公共流订阅方式会起作用，用户收到的数据会根据用户的选择方式而定。</td></tr>
    <tr><td bgcolor="Gainsboro">Q21. 用户重新登录行情服务器后，需要重新订阅么？</td></tr>
    <tr><td>A21. 需要。无论用户因为何种问题需要重新登录行情服务器，都需要重新订阅行情。</td></tr>
    <tr><td bgcolor="Gainsboro">Q22. 下单结构体EMTOrderInsertInfo中的order_client_id有什么作用？</td></tr>
    <tr><td>A22. order_client_id为用户自定义字段，用户下单时输入什么值，订单响应OnOrderEvent()返回时就会带回什么值，类似于备注，方便用户自己定位订单。当然，如果你什么都不填，也是可以的。合理的规划好order_client_id，将有助于用户更快定位订单。</td></tr>
    <tr><td bgcolor="Gainsboro">Q23. 股票代码ticker字段有什么要求？</td></tr>
    <tr><td>A23. ticker中要以’\0’结尾，并且不能带任何空格。</td></tr>
    <tr><td bgcolor="Gainsboro">Q24. 订单响应结构体EMTOrderInfo中的qty_left数量，为什么在撤单成功时不显示为0？</td></tr>
    <tr><td>A24. qty_left在订单为未成交、部成、全成、废单状态时，表示此订单还没有成交的数量，在部撤、全撤状态时，表示此订单被撤的数量。</td></tr>
    <tr><td bgcolor="Gainsboro">Q25. 为何没有撤单成功响应函数？</td></tr>
    <tr><td>A25. 对于撤单，如果成功响应了，原订单的状态肯定会变成部撤或者全撤，所以没有提供单独的撤单成功响应。只有撤单失败响应，告诉用户撤单失败的原因。</td></tr>
    <tr><td bgcolor="Gainsboro">Q26. GetAccountByEMTID只能在登录之后调用么？</td></tr>
    <tr><td>A26. 是的，只能在account登录后才能得到正确的结果。</td></tr>
    <tr><td bgcolor="Gainsboro">Q27. GetClientIDByEMTID有何作用？</td></tr>
    <tr><td>A27. 当多个客户端用同一个account登录时，可以通过此函数得到是哪个client_id的客户端下的单，并据此过滤出自己的订单。</td></tr>
    <tr><td bgcolor="Gainsboro">Q28. 一个api里，调用多次login函数登陆多个账户，下单的时候就根据session_id来区分单子下到哪个账户里，但是收到成交回报之后，怎么区分是哪个账号的成交回报呢？</td></tr>
    <tr><td>A28. 回调函数中增加了session_id参数可帮助用户进行区分，也可以通过GetAccountByEMTID来获取此订单对应的账号，但是建议用order_client_id来区分，规划好order_client_id会更方便快捷。</td></tr>
    <tr><td bgcolor="Gainsboro">Q29. 为何撤单失败提示错误，找不到原单？</td></tr>
    <tr><td>A29. 请检查撤单时传入的order_emt_id是否是正确的，order_emt_id是64位的，请确保传入的order_emt_id与下单时返回的order_emt_id是一致的。</td></tr>
    <tr><td bgcolor="Gainsboro">Q30. 为何有的时候下单后被拒，成为废单？</td></tr>
    <tr><td>A30. 请先根据拒单原因检查一下订单，可以按如下项进行检查：（1）如果买单，请检查报单数量，是否为100的整数倍，是否超过了100万股。（2）限价单的话，价格是否超过涨跌停价格。（3）价格类型是否正确，是否是交易所允许的价格类型。（4）股票代码是否正确，是否跟交易所类型匹配。（5）交易所类型是否正确。（6）是否下单数量过多，触发了风控。（7）是否撤单过多，触发了风控。（8）是否在允许报单时间内。（9）business_type是否设置正确。（10）position_effect是否设置正确。若以上都不是，请看一下是否是模拟撮合环境，如果是模拟撮合环境，会有一定几率发生模拟交易所拒单。此时交易所拒单的error_code是11110000，或者11100000，error_msg通常为217或者10000或者29999。这是模拟拒单，方便您测试拒单情况，不代表您的报单有错误。</td></tr>
    <tr><td bgcolor="Gainsboro">Q31. 在订单发生成交的时候，OrderEvent订单响应会有推送么？</td></tr>
    <tr><td>A31. OrderEvent只会在订单的开始状态，或者终止状态时推送，部成的时候不会推送，部成状态需要用户根据成交回报来确定。</td></tr>
    <tr><td bgcolor="Gainsboro">Q32. 为何成交回报TradeEvent中的成交数量，有的时候不是100的整数倍？</td></tr>
    <tr><td>A32. 成交回报的数量取决于卖单的数量，如果卖单不是100的整数倍，那么成交数量是有可能不是100的整数倍的。交易所允许最后一单卖单，可以下非100的整数倍。</td></tr>
    <tr><td bgcolor="Gainsboro">Q33. 全成的订单响应OrderEvent会在此订单的所有成交回报TradeEvent之后到达么？</td></tr>
    <tr><td>A33. 我们的系统确保订单的开始状态OrderEvent在此订单的所有成交回报到达之前到达，同时也确保订单的结束状态OrderEvent在此订单的所有成交回报之后到达。</td></tr>
    <tr><td bgcolor="Gainsboro">Q34. 每次login之后的session_id会变化么？</td></tr>
    <tr><td>A34. 是会变化的，所以请每次登录后及时更新session_id。</td></tr>
    <tr><td bgcolor="Gainsboro">Q35. 在查询资金接口中，为何总资产=可用资金+预扣资金？</td></tr>
    <tr><td>A35. 总资产total_asset=可用资金buying_power+证券资产security_asset+预扣资金withholding_amount，由于目前security_asset我们暂时不做统计，默认为0，因此总资产=可用资金+预扣资金。</td></tr>
    <tr><td bgcolor="Gainsboro">Q36. 客户如果剩余头寸为零股，可以按零股融券开仓吗？如果融券头寸只剩下50股可用了，那这50股都开仓不了，还收他头寸占用费用</td></tr>
    <tr><td>A36. 没有办法的事情.交易所规定最低交易股数是100股，但是这个股东账号是我们公司的，我们公司的专户数量你剩下50股，但是其他客户可能是整的</td></tr>
    <tr><td bgcolor="Gainsboro">Q37. 在部分成交的时候，OnOrderEvent不会推送部分成交（EMT_ORDER_STATUS_PARTTRADEDQUEUEING）？在什么时候会推EMT_ORDER_STATUS_PARTTRADEDQUEUEING？</td></tr>
    <tr><td>A37. 在查询报单响应时OnQueryOrder中推送</td></tr>
    <tr><td bgcolor="Gainsboro">Q38. 盘中订单手续费去哪里查询</td></tr>
    <tr><td>A38. 盘中是预扣手续费。资金查询，返回的预扣资金，包括购买股票未成交时预扣的交易资金+预扣手续费。实盘环境下以盘后清算为准。</td></tr>
    <tr><td bgcolor="Gainsboro">Q39. 服务器支持过夜单么？</td></tr>
    <tr><td>A39. 不支持过夜单。</td></tr>
    <tr><td bgcolor="Gainsboro">Q40. 如果有一只股票已经订阅过了，还没有退订，然后在某个地方又发起了订阅请求，这样会有什么影响吗？</td></tr>
    <tr><td>A40. 允许重复订阅，不影响。</td></tr>
    <tr><td bgcolor="Gainsboro">Q41. 请问逐笔成交里有些撤单价格为0是什么意思？</td></tr>
    <tr><td>A41. 因为报单时使用的是市价委托。</td></tr>
    <tr><td bgcolor="Gainsboro">Q42. 可以多账户登录吗？多账户时共用一个socket还是每个账户一个socket？</td></tr>
    <tr><td>A42. api支持多个账户连接，多次调用login即可，一个账户一个socket连接，不会共享连接。</td></tr>
    <tr><td bgcolor="Gainsboro">Q43. 请问getApiLastError这个函数是线程安全的吗？</td></tr>
    <tr><td>A43. 这个函数不是线程安全的。</td></tr>
    <tr><td bgcolor="Gainsboro">Q44. OnDisconnected()什么时候会调用？</td></tr>
    <tr><td>A44. 在服务器与客户端断线的时候会调用。如果此时用户想要重连，只需要在函数中再次调用Login()，并在登录成功后更新session_id即可。在用户主动调用Logout()时不会触发OnDisconnected()。</td></tr>
    <tr><td bgcolor="Gainsboro">Q45. API会自动断线重连么？</td></tr>
    <tr><td>A45. API不会主动帮助用户断线重连。当断线发生时，如果用户需要重连，只需在OnDisconnected()函数中再次调用Login()，并在登录成功后更新session_id即可。客户端此时会从上次收到的消息进行续传。</td></tr>
    <tr><td bgcolor="Gainsboro">Q46. OnError()什么时候调用？</td></tr>
    <tr><td>A46. 只有在服务器发生错误的时候才会触发OnError()。一般情况下，都不会触发。</td></tr>
    <tr><td bgcolor="Gainsboro">Q47. 融券费用怎么算？</td></tr>
    <tr><td>A47. 独占融券头寸全额占用费，可用公式：调拨融券数量×标的证券调拨日收盘价格×专用融券费率×占用天数/360</td></tr>
    <tr><td bgcolor="Gainsboro">Q48. 在查询持仓中股票名称为何是乱码？</td></tr>
    <tr><td>A48. 股票名称的编码是UTF-8，请检查一下编码是否正确。</td></tr>
    <tr><td bgcolor="Gainsboro">Q49. 在断线后，如何系统恢复？</td></tr>
    <tr><td>A49. 断线后，不重启的情况下，直接在OnDisconnected()函数中再次调用Login()，就默认是resume方式；如果是重启的情况下，只能通过restart方式，或者quick方式+查询。</td></tr>
    <tr><td bgcolor="Gainsboro">Q50. demo程序为何一直在下单？</td></tr>
    <tr><td>A50. demo测试程序采用的是乒乓测试下单，当收到OrderEvent时，会根据订单的状态来触发撤单或者新一轮下单。除了在emt_api_demo.cpp文件中有InsertOrder的调用，在trade_spi.cpp文件的OnOrderEvent()响应函数中，也有InsertOrder的调用。如果用demo测试下单时，发觉所有订单都是拒单，请参考（第30题）来解决问题。</td></tr>
    <tr><td bgcolor="Gainsboro">Q51. 在下单的时候，在接收回调函数通知时，会不会保证按照正常的订单生命周期对应的时间顺序；举个例子，全成会在部成后面出现，而不会是部成在全成后面出现。</td></tr>
    <tr><td>A51. 一般情况都是按时间推送的，部成不推送消息。</td></tr>
    <tr><td bgcolor="Gainsboro">Q52. 买卖队列的有效委托笔数和总委托笔数有什么区别呢？</td></tr>
    <tr><td>A52. 有效委托笔数是程序里能存储的数量，总委托笔数是市场里的委托数量。</td></tr>
    <tr><td bgcolor="Gainsboro">Q53. onOrderEvent中撤单时，返回的cancel_time这个时间指的是哪个时间，交易所返回的撤单时间？还是API接收到交易所返回撤单信息的时间。</td></tr>
    <tr><td>A53. 交易所时间。</td></tr>
    <tr><td bgcolor="Gainsboro">Q54. CreateQuoteApi和CreateTraderApi直接返回NULL是什么原因？</td></tr>
    <tr><td>A54. 请检查是否创建了过多的api对象，对于行情可以创建2个，对于交易只能创建1个；如果数量没有超过限制请检查输入参数是否有误，clientid要用正整数；还有一个可能的原因是库文件和头文件的版本不一致。</td></tr>
    <tr><td bgcolor="Gainsboro">Q55. API的spi里面的回调，是多线程执行还是单线程的？</td></tr>
    <tr><td>A55. 查询的都是一个线程，OnOrderEvent和OnTradeEvent这些可能会存在两个线程；Quotespi中，会有两个线程。</td></tr>
    <tr><td bgcolor="Gainsboro">Q56. 融资可用余额怎么查？</td></tr>
    <tr><td>A56. QueryCreditFundInfo可以使用这个接口查询。</td></tr>
    <tr><td bgcolor="Gainsboro">Q57. 如果发生除权，昨收价拿到的是除权前的价格还是除权后的价格？</td></tr>
    <tr><td>A57. 是除权后的价格。</td></tr>
    <tr><td bgcolor="Gainsboro">Q58. api的log是同步写的吗？</td></tr>
    <tr><td>A58. 是的。</td></tr>
    <tr><td bgcolor="Gainsboro">Q59. 下单时资金是怎么预扣的？</td></tr>
    <tr><td>A59. 如果是限价单，按报的价格预扣，如果是市价单，按涨停价预扣。</td></tr>
    <tr><td bgcolor="Gainsboro">Q60. 两融账户可以交易逆回购吗？</td></tr>
    <tr><td>A60.不可以。</td></tr>
    <tr><td bgcolor="Gainsboro">Q61. 报单时报单数量允许报非100的倍数么？</td></tr>
    <tr><td>A61. （1）如果是买单，是不允许的。（2）如果是卖单，如果持仓中有零散股，只允许将零散股一次性报出。例如持仓中A股票有232股，那么可以分100股、100股、32股报，也可以分100股、132股报，还可以232股报。</td></tr>
    <tr><td bgcolor="Gainsboro">Q62. 新股申购如何操作？</td></tr>
    <tr><td>A62. 可以根据QueryIPOInfoList()和QueryIPOQuotaInfo()查询出今日的新股信息和可申购额度，进行操作。</td></tr>
    <tr><td bgcolor="Gainsboro">Q63. 为什么我调用某些接口函数时，没有期望的spi函数调用？</td></tr>
    <tr><td>A63. 请检查您编译时连接的库版本和实际运行时链接的库版本是否一致，如果不一致，请更新一下。（可通过日志看到实际运行的版本号）</td></tr>
    <tr><td bgcolor="Gainsboro">Q64. 担保品划转是实时扣减数量么？</td></tr>
    <tr><td>A64. 对于担保品转出，会实时扣减数量。当收到订单确认消息，即视为转出成功。对于担保品转入，不会实时扣减。当收到订单确认消息时，不是真正的传入成功，需要等盘后清算时确认。</td></tr>
    <tr><td bgcolor="Gainsboro">Q65. 会发生InsertOrder()接口还没返回，但是先收到订单确认状态的情况么？</td></tr>
    <tr><td>A65. 会发生这种情况，当调用InsertOrder接口的线程被挂起时，可能会晚于订单确认消息返回。用户需注意收到不存在的订单时，不要丢弃。</td></tr>
</table>
