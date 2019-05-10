---
title: "Add vocabulary annotations to EdmEnumMember"
description: ""
category: "6. OData Features"
author: Khairunj
ms.author: Khairunj
ms.date: 02/19/2019
ms.topic: article
ms.service: multiple
---

From ODataLib 6.11.0, it supports to add vocabulary annotations to EdmEnumMember.

## Create Model

``` csharp
EdmModel model = new EdmModel();

EdmEntityContainer container = new EdmEntityContainer("DefaultNamespace", "Container");
model.AddElement(container);

EdmEntityType carType = new EdmEntityType("DefaultNamespace", "Car");
EdmStructuralProperty carOwnerId = carType.AddStructuralProperty("OwnerId", EdmCoreModel.Instance.GetInt32(false));
carType.AddKeys(carOwnerId);

var colorType = new EdmEnumType("DefaultNamespace", "Color", true);
model.AddElement(colorType);
colorType.AddMember("Cyan", new EdmIntegerConstant(1));
colorType.AddMember("Blue", new EdmIntegerConstant(2));

EdmEnumMember enumMember = new EdmEnumMember(colorType, "Red", new EdmIntegerConstant(3));
colorType.AddMember(enumMember);

EdmTerm stringTerm = new EdmTerm("DefaultNamespace", "StringTerm", EdmCoreModel.Instance.GetString(true));
model.AddElement(stringTerm);

var annotation = new EdmAnnotation(
    enumMember,
    stringTerm,
    "q1",
    new EdmStringConstant("Hello world!"));
    model.AddVocabularyAnnotation(annotation);

carType.AddStructuralProperty("Color", new EdmEnumTypeReference(colorType, false));

container.AddEntitySet("Cars", carType);
```

## Output

    <?xml version="1.0" encoding="utf-8"?>
      <edmx:Edmx Version="4.0" xmlns:edmx="https://docs.oasis-open.org/odata/ns/edmx">
        <edmx:DataServices>
          <Schema Namespace="DefaultNamespace" xmlns="https://docs.oasis-open.org/odata/ns/edm">
            <EnumType Name="Color" IsFlags="true">
              <Member Name="Cyan" Value="1" />
              <Member Name="Blue" Value="2" />
              <Member Name="Red" Value="3">
                <Annotation Term="DefaultNamespace.StringTerm" Qualifier="q1" String="Hello world!" />
              </Member>
            </EnumType>
            <Term Name="StringTerm" Type="Edm.String" />
            <EntityContainer Name="Container">
                <EntitySet Name="Cars" EntityType="DefaultNamespace.Car" />
            </EntityContainer>
          </Schema>
        </edmx:DataServices>
      </edmx:Edmx>