# Variable Example


```cpp
 // Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
//#include <Engine/Classes/Components/TextRenderCompnent.h>
#include "Components/TextRenderComponent.h"
#include "Countdown.generated.h"

UCLASS()
class TUTORIAL2_API ACountdown : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	ACountdown();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

	int32 CountdownTime;

	UTextRenderComponent* CountdownText;

	void UpdateTimerDisplay();
	void AdvancedTimer();
	void CountdownHasFinished();

	FTimerHandle CountdownTimerHandle;
};

```

```cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "Countdown.h"

// Sets default values
ACountdown::ACountdown()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = false;

	CountdownText = CreateDefaultSubobject<UTextRenderComponent>(TEXT("Countdown text"));
	CountdownText->SetHorizontalAlignment(EHTA_Center);
	CountdownText->SetWorldSize(150.0f);
	RootComponent = CountdownText;

	CountdownTime = 3;

}

// Called when the game starts or when spawned
void ACountdown::BeginPlay()
{
	Super::BeginPlay();
	UpdateTimerDisplay();
	GetWorldTimerManager().SetTimer(CountdownTimerHandle, this, &ACountdown::AdvancedTimer, 1.0f, true);
}

void ACountdown::UpdateTimerDisplay()
{
	//CountdownText->SetText(FString::FromInt(FMath::Max(CountdownTime, 0)));
	CountdownText->SetText(FText::FromString(FString::FromInt(FMath::Max(CountdownTime, 0))));
}


void ACountdown::AdvancedTimer()
{
	--CountdownTime;
	UpdateTimerDisplay();

	if (CountdownTime < 1)
	{
		GetWorldTimerManager().ClearTimer(CountdownTimerHandle);
		CountdownHasFinished();
	}
}

void ACountdown::CountdownHasFinished()
{
	CountdownText->SetText(FText::FromString(TEXT("Go!")));

}
```

Expose member variavle to editor. use `UPROPERRY(EditAnywhere)`

```cpp
public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

	UPROPERRY(EditAnywhere)

	int32 CountdownTime;
```