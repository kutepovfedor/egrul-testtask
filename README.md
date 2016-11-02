# Test project: EGRUL database

## Functional requiriments

### User-stories
#### Import data
1. Application starts with EGRUL-files destination directory
1. Application read the directory and decide what zips are not loaded yet (new)
1. Application for each new zip-file:
   1. Unzip it
   1. Retrieve XMLs
   1. For every XML:
      1. Split in СвЮЛ nodes
      1. For every СвЮЛ node:
         1. Store СвЮл node translated in JSON format in Cassandra database
         1. Update Elasticsearch database with new СвЮл data
#### Data access
1. Retrieve a company information
   1. for a concrete `ОГРН` with last `ДатаВып`
   1. for a concrete `ОГРН` for all `ДатаВып` sorted by `ДатаВып`

### Formats
#### EGRUL xml format:
```
<EGRUL ДатаВыг="2015-09-12">
    <СвЮЛ ДатаВып="2015-09-11" ОГРН="1147901001863" ДатаОГРН="2014-11-25" ИНН="7901544834" КПП="790101001" СпрОПФ="ОКОПФ" КодОПФ="12300" ПолнНаимОПФ="ОБЩЕСТВА С ОГРАНИЧЕННОЙ ОТВЕТСТВЕННОСТЬЮ">
        <СвНаимЮЛ НаимЮЛПолн="ОБЩЕСТВО С ОГРАНИЧЕННОЙ ОТВЕТСТВЕННОСТЬЮ "СЛУЖБА ЕДИНОГО ЗАКАЗЧИКА"" НаимЮЛСокр="ООО "СЛУЖБА ЕДИНОГО ЗАКАЗЧИКА"">
            <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
        </СвНаимЮЛ>
        <СвАдресЮЛ>
            <АдресРФ Индекс="679000" КодРегион="79" КодАдрКладр="790000010000266" Дом="79" Корпус="А" Кварт="ОФИС 4">
                <Регион ТипРегион="АВТОНОМНАЯ ОБЛАСТЬ" НаимРегион="ЕВРЕЙСКАЯ"/>
                <Город ТипГород="ГОРОД" НаимГород="БИРОБИДЖАН"/>
                <Улица ТипУлица="УЛИЦА" НаимУлица="ШОЛОМ-АЛЕЙХЕМА"/>
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </АдресРФ>
        </СвАдресЮЛ>
        <СвОбрЮЛ ОГРН="1147901001863" ДатаОГРН="2014-11-25">
            <СпОбрЮЛ КодСпОбрЮЛ="11" НаимСпОбрЮЛ="ГОСУДАРСТВЕННАЯ РЕГИСТРАЦИЯ ЮРИДИЧЕСКОГО ЛИЦА ПРИ СОЗДАНИИ"/>
            <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
        </СвОбрЮЛ>
        <СвРегОрг КодНО="7901" НаимНО="ИНСПЕКЦИЯ ФЕДЕРАЛЬНОЙ НАЛОГОВОЙ СЛУЖБЫ ПО Г.БИРОБИДЖАНУ ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ" АдрРО=",679000,ЕВРЕЙСКАЯ АОБЛ,, БИРОБИДЖАН Г,, КОМСОМОЛЬСКАЯ УЛ,11, А,">
            <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
        </СвРегОрг>
        <СвУчетНО ИНН="7901544834" КПП="790101001" ДатаПостУч="2014-11-25">
            <СвНО КодНО="7901" НаимНО="ИНСПЕКЦИЯ ФЕДЕРАЛЬНОЙ НАЛОГОВОЙ СЛУЖБЫ ПО Г.БИРОБИДЖАНУ ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ"/>
            <ГРНДата ГРН="2147901023543" ДатаЗаписи="2014-11-25"/>
        </СвУчетНО>
        <СвРегФСС РегНомФСС="790000757179001" ДатаРег="2014-12-20">
            <СвОргФСС КодФСС="7900" НаимФСС="ГОСУДАРСТВЕННОЕ УЧРЕЖДЕНИЕ - РЕГИОНАЛЬНОЕ ОТДЕЛЕНИЕ ФОНДА СОЦИАЛЬНОГО СТРАХОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ ПО ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ"/>
            <ГРНДата ГРН="2157901051097" ДатаЗаписи="2015-09-11"/>
        </СвРегФСС>
        <СвУстКап НаимВидКап="УСТАВНЫЙ КАПИТАЛ" СумКап="10000.0000">
            <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
        </СвУстКап>
        <СведДолжнФЛ>
            <СвФЛ Фамилия="АВЕРИНА" Имя="ОЛЬГА" Отчество="ВИКТОРОВНА" ИННФЛ="790100828605">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвФЛ>
            <СвДолжн ВидДолжн="02" НаимВидДолжн="РУКОВОДИТЕЛЬ ЮРИДИЧЕСКОГО ЛИЦА" НаимДолжн="ГЕНЕРАЛЬНЫЙ ДИРЕКТОР">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвДолжн>
        </СведДолжнФЛ>
        <СвУчредит>
            <УчрФЛ>
                <СвФЛ Фамилия="ГУБАНОВ" Имя="СЕРГЕЙ" Отчество="НИКОЛАЕВИЧ" ИННФЛ="790105830171">
                    <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
                </СвФЛ>
                <ДоляУстКап НоминСтоим="10000.0000">
                    <РазмерДоли>
                        <Процент>100</Процент>
                    </РазмерДоли>
                    <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
                </ДоляУстКап>
            </УчрФЛ>
        </СвУчредит>
        <СвОКВЭД>
            <СвОКВЭДОсн КодОКВЭД="74.11" НаимОКВЭД="ДЕЯТЕЛЬНОСТЬ В ОБЛАСТИ ПРАВА">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДОсн>
            <СвОКВЭДДоп КодОКВЭД="74.12" НаимОКВЭД="ДЕЯТЕЛЬНОСТЬ В ОБЛАСТИ БУХГАЛТЕРСКОГО УЧЕТА И АУДИТА">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДДоп>
            <СвОКВЭДДоп КодОКВЭД="74.13" НаимОКВЭД="ИССЛЕДОВАНИЕ КОНЪЮНКТУРЫ РЫНКА И ВЫЯВЛЕНИЕ ОБЩЕСТВЕННОГО МНЕНИЯ">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДДоп>
            <СвОКВЭДДоп КодОКВЭД="74.14" НаимОКВЭД="КОНСУЛЬТИРОВАНИЕ ПО ВОПРОСАМ КОММЕРЧЕСКОЙ ДЕЯТЕЛЬНОСТИ И УПРАВЛЕНИЯ">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДДоп>
            <СвОКВЭДДоп КодОКВЭД="74.84" НаимОКВЭД="ПРЕДОСТАВЛЕНИЕ ПРОЧИХ УСЛУГ">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДДоп>
            <СвОКВЭДДоп КодОКВЭД="74.40" НаимОКВЭД="РЕКЛАМНАЯ ДЕЯТЕЛЬНОСТЬ">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДДоп>
            <СвОКВЭДДоп КодОКВЭД="74.50" НаимОКВЭД="НАЙМ РАБОЧЕЙ СИЛЫ И ПОДБОР ПЕРСОНАЛА">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДДоп>
            <СвОКВЭДДоп КодОКВЭД="74.15" НаимОКВЭД="ДЕЯТЕЛЬНОСТЬ ПО УПРАВЛЕНИЮ ФИНАНСОВО-ПРОМЫШЛЕННЫМИ ГРУППАМИ И  ХОЛДИНГ-КОМПАНИЯМИ">
                <ГРНДата ГРН="1147901001863" ДатаЗаписи="2014-11-25"/>
            </СвОКВЭДДоп>
        </СвОКВЭД>
        <СвЗапЕГРЮЛ ИдЗап="1147901001863" ГРН="1147901001863" ДатаЗап="2014-11-25">
            <ВидЗап КодСПВЗ="11201" НаимВидЗап="ГОСУДАРСТВЕННАЯ РЕГИСТРАЦИЯ ЮРИДИЧЕСКОГО ЛИЦА ПРИ СОЗДАНИИ"/>
            <СвРегОрг КодНО="7901" НаимНО="ИНСПЕКЦИЯ ФЕДЕРАЛЬНОЙ НАЛОГОВОЙ СЛУЖБЫ ПО Г.БИРОБИДЖАНУ ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ"/>
            <СведПредДок>
                <НаимДок>ЗАЯВЛЕНИЕ О ГОСУДАРСТВЕННОЙ РЕГИСТРАЦИИ ЮРИДИЧЕСКОГО ЛИЦА ПРИ СОЗДАНИИ</НаимДок>
                <НомДок>783</НомДок>
            </СведПредДок>
            <СведПредДок>
                <НаимДок>УСТАВ</НаимДок>
            </СведПредДок>
            <СведПредДок>
                <НаимДок>РЕШЕНИЕ О СОЗДАНИИ ЮРИДИЧЕСКОГО ЛИЦА</НаимДок>
                <НомДок>1</НомДок>
                <ДатаДок>2014-11-05</ДатаДок>
            </СведПредДок>
            <СведПредДок>
                <НаимДок>ДОКУМЕНТ ОБ УПЛАТЕ ГОСУДАРСТВЕННОЙ ПОШЛИНЫ</НаимДок>
                <НомДок>55</НомДок>
                <ДатаДок>2014-11-18</ДатаДок>
            </СведПредДок>
            <СвСвид Серия="79" Номер="000310743" ДатаВыдСвид="2014-11-25"/>
        </СвЗапЕГРЮЛ>
        <СвЗапЕГРЮЛ ИдЗап="2147901023543" ГРН="2147901023543" ДатаЗап="2014-11-25">
            <ВидЗап КодСПВЗ="13200" НаимВидЗап="ВНЕСЕНИЕ В ЕДИНЫЙ ГОСУДАРСТВЕННЫЙ РЕЕСТР ЮРИДИЧЕСКИХ ЛИЦ СВЕДЕНИЙ ОБ УЧЕТЕ ЮРИДИЧЕСКОГО ЛИЦА В НАЛОГОВОМ ОРГАНЕ"/>
            <СвРегОрг КодНО="7901" НаимНО="ИНСПЕКЦИЯ ФЕДЕРАЛЬНОЙ НАЛОГОВОЙ СЛУЖБЫ ПО Г.БИРОБИДЖАНУ ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ"/>
        </СвЗапЕГРЮЛ>
        <СвЗапЕГРЮЛ ИдЗап="2157901049557" ГРН="2157901049557" ДатаЗап="2015-09-11">
            <ВидЗап КодСПВЗ="13400" НаимВидЗап="ВНЕСЕНИЕ В ЕДИНЫЙ ГОСУДАРСТВЕННЫЙ РЕЕСТР ЮРИДИЧЕСКИХ ЛИЦ СВЕДЕНИЙ О РЕГИСТРАЦИИ ЮРИДИЧЕСКОГО ЛИЦА В КАЧЕСТВЕ СТРАХОВАТЕЛЯ В ИСПОЛНИТЕЛЬНОМ ОРГАНЕ ФОНДА СОЦИАЛЬНОГО СТРАХОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ"/>
            <СвРегОрг КодНО="7901" НаимНО="ИНСПЕКЦИЯ ФЕДЕРАЛЬНОЙ НАЛОГОВОЙ СЛУЖБЫ ПО Г.БИРОБИДЖАНУ ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ"/>
        </СвЗапЕГРЮЛ>
        <СвЗапЕГРЮЛ ИдЗап="2157901050965" ГРН="2157901050965" ДатаЗап="2015-09-11">
            <ВидЗап КодСПВЗ="13400" НаимВидЗап="ВНЕСЕНИЕ В ЕДИНЫЙ ГОСУДАРСТВЕННЫЙ РЕЕСТР ЮРИДИЧЕСКИХ ЛИЦ СВЕДЕНИЙ О РЕГИСТРАЦИИ ЮРИДИЧЕСКОГО ЛИЦА В КАЧЕСТВЕ СТРАХОВАТЕЛЯ В ИСПОЛНИТЕЛЬНОМ ОРГАНЕ ФОНДА СОЦИАЛЬНОГО СТРАХОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ"/>
            <СвРегОрг КодНО="7901" НаимНО="ИНСПЕКЦИЯ ФЕДЕРАЛЬНОЙ НАЛОГОВОЙ СЛУЖБЫ ПО Г.БИРОБИДЖАНУ ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ"/>
        </СвЗапЕГРЮЛ>
        <СвЗапЕГРЮЛ ИдЗап="2157901051097" ГРН="2157901051097" ДатаЗап="2015-09-11">
            <ВидЗап КодСПВЗ="13400" НаимВидЗап="ВНЕСЕНИЕ В ЕДИНЫЙ ГОСУДАРСТВЕННЫЙ РЕЕСТР ЮРИДИЧЕСКИХ ЛИЦ СВЕДЕНИЙ О РЕГИСТРАЦИИ ЮРИДИЧЕСКОГО ЛИЦА В КАЧЕСТВЕ СТРАХОВАТЕЛЯ В ИСПОЛНИТЕЛЬНОМ ОРГАНЕ ФОНДА СОЦИАЛЬНОГО СТРАХОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ"/>
            <СвРегОрг КодНО="7901" НаимНО="ИНСПЕКЦИЯ ФЕДЕРАЛЬНОЙ НАЛОГОВОЙ СЛУЖБЫ ПО Г.БИРОБИДЖАНУ ЕВРЕЙСКОЙ АВТОНОМНОЙ ОБЛАСТИ"/>
        </СвЗапЕГРЮЛ>
    </СвЮЛ>
```

#### EGRUL JSON format
**What JSON format use???**
```
???
```

## Technical requirements

### Cassandra
* Is there any sense for customer in Ansible scrips?
  If there is a sense:
  1. OS: distributive, version?
  1. Cassandra: distributive, version?

### Elasticsearch
Elasticsearch (out of scope): the customer has already Elasticsearch cluster installed
